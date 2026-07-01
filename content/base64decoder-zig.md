+++
title = "Base64 Decoder written in Zig"
description = ""
date = 2024-04-23
updated = 2026-06-30
draft = false

[taxonomies]
tags = ["zig", "base64", "encoding"]

[extra]
#images = [""]
status = "Complete"
source = "https://codeberg.org/oraqlle/base64-decoder"
+++

Very simple [Base64](https://en.wikipedia.org/wiki/Base64) decoder written in Zig,
created during my university days. No idea if it still works given the unstable nature of
Zig pre-v1.

```zig
const std = @import("std");

const B64DecodeMap = [_]u6{
    //  binary,     char, idx, val
    0b000000, // NUL, 0
    0b000000, // SOH, 1
    0b000000, // STX, 2
    0b000000, // ETX, 3
    0b000000, // EOQ, 4
    0b000000, // ENQ, 5
    0b000000, // ACK, 6
    0b000000, // BEL, 7
    0b000000, // BS, 8
    0b000000, // HT, 9
    0b000000, // LF, 10
    0b000000, // VT, 11
    0b000000, // FF, 12
    0b000000, // CR, 13
    0b000000, // SO, 14
    0b000000, // SI, 15
    0b000000, // DLE, 16
    0b000000, // DC1, 17
    0b000000, // DC2, 18
    0b000000, // DC3, 19
    0b000000, // DC4, 20
    0b000000, // NAK, 21
    0b000000, // SYN, 22
    0b000000, // ETB, 23
    0b000000, // CAN, 24
    0b000000, // EM, 25
    0b000000, // SUB, 26
    0b000000, // ESC, 27
    0b000000, // FS, 28
    0b000000, // GS, 29
    0b000000, // RS, 30
    0b000000, // US, 31
    0b000000, // SP, 32
    0b000000, // !, 33
    0b000000, // ", 34
    0b000000, // #, 35
    0b000000, // $, 36
    0b000000, // %, 37
    0b000000, // &, 38
    0b000000, // ', 39
    0b000000, // (, 40
    0b000000, // ), 41
    0b000000, // *, 42
    0b111110, // +, 43, 62
    0b000000, // ,, 44
    0b000000, // -, 45
    0b000000, // ., 46
    0b111111, // /, 47, 63
    0b110100, // 0, 48, 52
    0b110101, // 1, 49, 53
    0b110110, // 2, 50, 54
    0b110111, // 3, 51, 55
    0b111000, // 4, 52, 56
    0b111001, // 5, 53, 57
    0b111010, // 6, 54, 58
    0b111011, // 7, 55, 59
    0b111100, // 8, 56, 60
    0b111101, // 9, 57, 61
    0b000000, // :, 58
    0b000000, // ;, 59
    0b000000, // <, 60
    0b000000, // =, 61
    0b000000, // >, 62
    0b000000, // ?, 63
    0b000000, // @, 64
    0b000000, // A, 65, 0
    0b000001, // B, 66, 1
    0b000010, // C, 67, 2
    0b000011, // D, 68, 3
    0b000100, // E, 69, 4
    0b000101, // F, 70, 5
    0b000110, // G, 71, 6
    0b000111, // H, 72, 7
    0b001000, // I, 73, 8
    0b001001, // J, 74, 9
    0b001010, // K, 75, 10
    0b001011, // L, 76, 11
    0b001100, // M, 77, 12
    0b001101, // N, 78, 13
    0b001110, // O, 79, 14
    0b001111, // P, 80, 15
    0b010000, // Q, 81, 16
    0b010001, // R, 82, 17
    0b010010, // S, 83, 18
    0b010011, // T, 84, 19
    0b010100, // U, 85, 20
    0b010101, // V, 86, 21
    0b010110, // W, 87, 22
    0b010111, // X, 88, 23
    0b011000, // Y, 89, 24
    0b011001, // Z, 90, 25
    0b000000, // [, 91
    0b000000, // \, 92
    0b000000, // ], 93
    0b000000, // ^, 94
    0b000000, // _, 95
    0b000000, // `, 96
    0b011010, // a, 97, 26
    0b011011, // b, 98, 27
    0b011100, // c, 99, 28
    0b011101, // d, 100, 29
    0b011110, // e, 101, 30
    0b011111, // f, 102, 31
    0b100000, // g, 103, 32
    0b100001, // h, 104, 33
    0b100010, // i, 105, 34
    0b100011, // j, 106, 35
    0b100100, // k, 107, 36
    0b100101, // l, 108, 37
    0b100110, // m, 109, 38
    0b100111, // n, 110, 39
    0b101000, // o, 111, 40
    0b101001, // p, 112, 41
    0b101010, // q, 113, 42
    0b101011, // r, 114, 43
    0b101100, // s, 115, 44
    0b101101, // t, 116, 45
    0b101110, // u, 117, 46
    0b101111, // v, 118, 47
    0b110000, // w, 119, 48
    0b110001, // x, 120, 49
    0b110010, // y, 121, 50
    0b110011, // z, 122, 51
    0b000000, // {, 123
    0b000000, // |, 124
    0b000000, // }, 125
    0b000000, // ~, 126
    0b000000, // DEL, 127
};

pub fn base64Decode(alloc: std.mem.Allocator, chars: []const u8) !std.ArrayList(u8) {
    const hasPadding = chars.len % 4 != 0 or chars[chars.len - 1] == '=';
    const morePadding = hasPadding and ((chars.len % 4 > 2) or (chars[chars.len - 2] != '='));

    const numBytes = ((chars.len - @intFromBool(hasPadding)) / 4) << 2;
    const numOutChars = ((numBytes / 4) * 3) + @intFromBool(hasPadding) + @intFromBool(morePadding);

    var decodedStr = try std.ArrayList(u8).initCapacity(alloc, numOutChars + @intFromBool(morePadding));

    var byteIdx: u32 = 0;
    while (byteIdx < numBytes) : (byteIdx += 4) {
        const byte0: u24 = B64DecodeMap[chars[byteIdx]];
        const byte1: u24 = B64DecodeMap[chars[byteIdx + 1]];
        const byte2: u24 = B64DecodeMap[chars[byteIdx + 2]];
        const byte3: u24 = B64DecodeMap[chars[byteIdx + 3]];

        const bin = byte0 << 18 | byte1 << 12 | byte2 << 6 | byte3;

        try decodedStr.append(@truncate(bin >> 16));
        try decodedStr.append(@truncate(bin >> (8 & 0xFF)));
        try decodedStr.append(@truncate(bin & 0xFF));
    }

    if (hasPadding) {
        const pad0: u24 = B64DecodeMap[chars[numBytes]];
        const pad1: u24 = B64DecodeMap[chars[numBytes + 1]];

        var bin: u24 = pad0 << 18 | pad1 << 12;

        try decodedStr.append(@truncate(bin >> 16));

        if (morePadding) {
            const pad2: u24 = B64DecodeMap[chars[numBytes + 2]];
            bin |= pad2 << 6;
            try decodedStr.append(@truncate(bin >> 8 & 0xFF));
        }
    }

    return decodedStr;
}

pub fn main() !void {
    const stdout = std.io.getStdOut().writer();
    const stderr = std.io.getStdErr().writer();

    const argv = std.os.argv;

    var gpa = std.heap.GeneralPurposeAllocator(.{}){};
    defer {
        const gpa_status = gpa.deinit();
        if (gpa_status == .leak) std.testing.expect(false) catch @panic("Memory leak");
    }

    if (argv.len != 2) {
        try stderr.print("Expected 2 arguments, found: {d}\n", .{argv.len});
        std.process.exit(1);
    }

    const encodedData = std.mem.span(argv[1]);

    if (encodedData.len < -1) {
        try stderr.print("Argument needs to be five characters long, was: {d}\n", .{encodedData.len});
        std.process.exit(1);
    }

    const decodedStr = try base64Decode(gpa.allocator(), encodedData);
    defer decodedStr.deinit();

    try stdout.print("{s}", .{decodedStr.items[0..]});
}
```
