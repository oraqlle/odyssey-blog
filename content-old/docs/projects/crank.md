+++
title = "Crank Framework"
description = "A state management framework."
date = 2023-03-17T19:42:00+00:00
updated = 2023-06-22T16:00:00+00:00
draft = false
sort_by = "title"
template = "docs/page.html"

[extra]
lead = "A simple state management framework."
toc = true
top = false
+++

Crank is a simple state management framework built in vanilla C++. It allows you to create states that act like pages of an application and provides means to transition between pages.

Crank is designed to by as simple as possible and only provides a runtime framework for managing states. It is not designed to be able to render images/displays or handle other forms of processing. This is implemented by the user by whatever means they choose.

Crank is built using the [bpt](https://bpt.pizza) build tool and is hosted here.

- [GitHub](https://github.com/oraqlle/crank)
- [Docs - 🚧 Under Construction 🚧](...)

---

## Design and Use

This framework has two major components, the runtime engine which manages the state-stack and flow of operations and the states.

Crank uses the _Input_, _Update_, _Output_ (IUO) model to run a program. In crank these are methods called from a `crank::engine` object which dispatches to the current state. These methods are named called `handle_events()`, `update()`, and `render()` respectively.

- `handle_events()` - Handles events such as keyboard input.
- `update()` - Updates the internal state of a given state object
- `render()` - Outputs the updated state such as rendering to the screen or output to a console.

An example of a simple running program can be found on the next page.

### The Runtime Engine

The `crank::engine` class is very simple. It's main job is to manage the stack of states and provide an interface for calling the various methods of different states. States are created as `std::shared_ptr` and are created from factory functions registered to the engine. States are registered to the engine using an ID. This allows for different states to be register and spawned simply using the ID. When registering state, you can forward arguments that will be used by the state constructor. This is how resources are shared between states.

A new state is pushed to the stack using the `push_state()` and `pop_state()` methods with `push_state()` taking the corresponding state ID. There is also the `change_state()` method which can change the current state to a new one using the new states ID. Popping and changing state will result in the state being cleaned up by the engine.

The engine class also features some _meta-methods_ that help diagnose the state of the engine. Two such methods are the  `running()` method which indicates whether engine is currently running or if it is in the quit phase and the `quit()` method which shuts down the current engine and leaves it in its pre-init phase. This allows you to run a continues loop while the engine is running and have a state manage if the engine should quit.

### States

The `crank::states::state_interface` class is a pure virtual class design to blueprint the interface that a state should provide. These are only the minimum requirements for the engine to use the state, in derived state classes, you can implement other methods for handle other operations specific to that states processing.

The main methods provided by `crank::states::state_interface` are the `handle_events()`, `update()` and `draw()` methods. These methods are called by the engine when the corresponding engine methods are called. They are used to handle the main processes that can occur in a game or program.

The base class provides and interface for a `init()` method; called whenever the state is pushed to the engines state-stack and a `cleanup()` method called whenever the state is changed from the engines state-stack.

A few of the methods take a reference to the engine object that called them. These include; `init()`, `handle_events()`, `update()` and `draw()`. This is so you can access a particular engine resource or created your own shared reference.

A full implementation is available [below](#example).

The base class also interfaces a `pause()` and `resume()` methods which are used to freeze a state in its current state or unfreeze it to resume processing respectively. These methods are called when a new state is pushed on top of the current state or is popped off the top.

The final requirement for a derived state class is that the default constructor should be _defaulted_ (i.e. `base() = default`) and should be a protected member.

---

## Example

Here I will give a full implementation of a basic state and a main function that runs the engine for a few loop iterations. The state will be called `basic` and will be injected in the `crank::states` namespace. It is **_not_** recommended to inject your own states into this namespace.

### A simple state

Here I have implemented the interface for the `basic` class. The main thing to notice is the use of the static self member and the `instance()` method.

```cpp
/// -*- C++ -*- Header compatibility <basic.hxx>

/// \brief 
/// \file basic.hxx
///
/// author: Tyler Swann (tyler.swann05@gmail.com)
///
/// version: 0.2.0
///
/// date: 28-02-2023
///
/// copyright: Copyright (c) 2022-2023
///
/// license: MIT

#ifndef BASIC_HPP
#   define BASIC_HPP 1

#include <crank/crank.hxx>

#include <string>


/// \brief crank::states namespace
namespace crank::states
{

    /// \brief Basic state
    ///
    /// \details A basic state class designed
    /// to showcase how a state might look.
    ///
    /// \note In the source definition of this state, 
    /// you have to define a `crank::states::<state>` 
    /// object at the top of the file, (below that 
    /// #include directives is ideal and outside the 
    /// `crank::states` namespace).
    /// eg.
    /// ```cpp
    /// crank::states::basic crank::states::basic::m_basic;
    /// ```
    class basic 
        : public state_interface
    {
    public:

        explicit basic(int n, std::string msg) noexcept;

        void init(crank::engine& eng) noexcept;

        void cleanup() noexcept;

        void pause() noexcept;

        void resume() noexcept;

        void handle_events(crank::engine& eng) noexcept;

        void update(crank::engine& eng) noexcept;

        void render(crank::engine& eng) noexcept;

        /// \brief Get the state instance.
        ///
        /// \details Returns a static pointer to 
        /// the state instance held by this state. 
        // static basic& instance()
        // { return m_basic; }

    protected:

        /// \brief Protected Default Constructor
        basic() = default;
    
    private:

        /// \brief Static instance of the this 
        /// state type.
        // static basic m_basic;

        int m_i;
        std::string m_msg;

    }; /// class basic

} /// namespace crank::states

#endif /// BASIC_HPP
```

The implementation is nothing special, it just prints the current method so it can be seen what is being called and when.

```cpp
#include <example/basic.hxx>

#include <iostream>

namespace crank::states
{   
    basic::basic(int n, std::string msg) noexcept
        : m_i { n }
        , m_msg { msg }
    { }

    void basic::init([[maybe_unused]] crank::engine& eng) noexcept
    {
        std::clog << "i = " << m_i << std::endl;
        std::clog << "msg = " << m_msg << std::endl;
        std::clog << m_msg + " - basic::init()" << std::endl;
        m_i += 1;

        std::clog << "i = " << m_i << std::endl;
    }

    void basic::cleanup() noexcept
    { std::clog << m_msg + " - basic::cleanup()" << std::endl; }

    void basic::pause() noexcept
    { std::clog << m_msg + " - basic::pause()" << std::endl; }

    void basic::resume() noexcept
    { std::clog << m_msg + " - basic::resume()" << std::endl; }

    void basic::handle_events([[maybe_unused]] crank::engine& eng) noexcept
    { 
        std::clog << m_msg + " - basic::handle_events() with i: " << m_i << std::endl; 
        m_i += 1;
    }

    void basic::update([[maybe_unused]] crank::engine& eng) noexcept
    { 
        std::clog << m_msg + " - basic::update() with i: " << m_i << std::endl;
        m_i += 1;
    }

    void basic::render([[maybe_unused]] crank::engine& eng) noexcept
    { 
        std::clog << m_msg + " - basic::render() with i: " << m_i << std::endl;
        m_i += 1; 
    }

} /// namespace crank
```

### Using the engine

```cpp
#include <crank/crank.hxx>
#include <example/basic.hxx>

#include <iostream>
#include <string>

using namespace std::literals;

/// State ID's (user side)
enum id { Basic1, Basic2 };

auto main() -> int
{
    /// Create an engine
    auto engine = crank::engine{};

    /// Register Two different `Basic` states.
    /// You must register the type of the state
    /// as a template type parameter. We can also
    /// forward values to be used in the construction
    /// of the states. These arguments must follow 
    /// the state ID. 
    ///
    /// \note: The ID of a state is just an `int` 
    /// thus, you can use regular enums as ID's 
    /// to help decern ID's.
    engine.make_factory_for<crank::states::basic>(id::Basic1, 7, "Basic 1"s);
    engine.make_factory_for<crank::states::basic>(id::Basic2, 55, "Basic 2"s);

    /// Launch `Basic1` by changing state.
    engine.change_state(id::Basic1);

    std::cout << "---------------------------" << std::endl;

    /// Run full loop four times.
    auto i { 0 }; 
    while (i < 4 && engine.running())
    {
        engine.handle_events();
        engine.update();
        engine.render();
        std::cout << "loops: " << i++ << "\n---------------------------" << std::endl;
    }

    /// Push new state, `Basic2` to stack.
    /// `Basic2` is now the current state.
    engine.push_state(id::Basic2);

    std::cout << "---------------------------" << std::endl;

    /// Run full loop four more times
    /// for new state.
    while (i < 8 && engine.running())
    {
        engine.handle_events();
        engine.update();
        engine.render();
        std::cout << "loops: " << i++ << "\n---------------------------" << std::endl;
    }

    /// Pop top state (`Basic2`) from stack.
    /// `Basic1` is once again the current state.
    engine.pop_state();

    /// Run full loop four final times.
    while (i < 12 && engine.running())
    {
        engine.handle_events();
        engine.update();
        engine.render();
        std::cout << "loops: " << i++ << "\n---------------------------" << std::endl;
    }

    /// Force the engine to quit.
    engine.quit();

    return 0;
}
```

Output:

```sh
$ ./build/example/game
i = 7
msg = Basic 1
Basic 1 - basic::init()
i = 8
---------------------------
Basic 1 - basic::handle_events() with i: 8
Basic 1 - basic::update() with i: 9
Basic 1 - basic::render() with i: 10
loops: 0
---------------------------
Basic 1 - basic::handle_events() with i: 11
Basic 1 - basic::update() with i: 12
Basic 1 - basic::render() with i: 13
loops: 1
---------------------------
Basic 1 - basic::handle_events() with i: 14
Basic 1 - basic::update() with i: 15
Basic 1 - basic::render() with i: 16
loops: 2
---------------------------
Basic 1 - basic::handle_events() with i: 17
Basic 1 - basic::update() with i: 18
Basic 1 - basic::render() with i: 19
loops: 3
---------------------------
Basic 1 - basic::pause()
i = 55
msg = Basic 2
Basic 2 - basic::init()
i = 56
---------------------------
Basic 2 - basic::handle_events() with i: 56
Basic 2 - basic::update() with i: 57
Basic 2 - basic::render() with i: 58
loops: 4
---------------------------
Basic 2 - basic::handle_events() with i: 59
Basic 2 - basic::update() with i: 60
Basic 2 - basic::render() with i: 61
loops: 5
---------------------------
Basic 2 - basic::handle_events() with i: 62
Basic 2 - basic::update() with i: 63
Basic 2 - basic::render() with i: 64
loops: 6
---------------------------
Basic 2 - basic::handle_events() with i: 65
Basic 2 - basic::update() with i: 66
Basic 2 - basic::render() with i: 67
loops: 7
---------------------------
Basic 2 - basic::cleanup()
Basic 1 - basic::resume()
Basic 1 - basic::handle_events() with i: 20
Basic 1 - basic::update() with i: 21
Basic 1 - basic::render() with i: 22
loops: 8
---------------------------
Basic 1 - basic::handle_events() with i: 23
Basic 1 - basic::update() with i: 24
Basic 1 - basic::render() with i: 25
loops: 9
---------------------------
Basic 1 - basic::handle_events() with i: 26
Basic 1 - basic::update() with i: 27
Basic 1 - basic::render() with i: 28
loops: 10
---------------------------
Basic 1 - basic::handle_events() with i: 29
Basic 1 - basic::update() with i: 30
Basic 1 - basic::render() with i: 31
loops: 11
---------------------------
Basic 1 - basic::cleanup()
```

---

## Example Projects

Using Crank (along with SFML), I recreated Pong

- [Pong - Project Page](../pong/)
- [Pong - GitHub](https://github.com/oraqlle/pong)
