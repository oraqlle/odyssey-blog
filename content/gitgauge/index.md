+++
title = "gitgauge"
description = "Git Contribution Analytics for University Teachers"
date = 2025-10-24
updated = 2026-07-01
draft = false

[taxonomies]
tags = ["rust", "svelte", "git", "javascript", "tauri", "application", "desktop"]

[extra]
images = ["search-query.png", "loading-screen.png", "repo-overview.png", "expanded-graph.png"]
status = "Complete"
source = "https://github.com/Monash-FIT3170/2025W1-Commitment"
+++

**gitgauge** is a native desktop application built with Tauri, utilising **Svelte** for
the GUI and Rust for the main application logic. It was designed to help teachers at
universities holistically assess student contributions to a software project (provided
the project was a Git repository).

The application works by performing a bare clone of a repository into a special cache
directory so analysis can be done locally rather than constantly querying online source
host providers. This allowed us to automatically support any current and future git
hosting platforms as the program uses Git directly, not each services API which is
subject to change, meaning this application will work in its current form for a long time
(well as long as Git's fundamental pipeline for cloning repositories and storing metadata
data doesn't change in a breaking way).

The application has a graph plotting each contributor on a mean/median distribution with
options to select a particular branch, date range, commit message regex pattern.
You can even upload a configuration file to map different emails to the same contributor.

There are also summary cards to quickly show the number of additions and deletion of a
particular contributor along with the number of changes per commit on average and some
other metrics.

gitgauge was developed over the course of a year in a group of 12 students as part of our
university studies. I was in charge of designing the architecture of the app and ensuring
the UI and the logic layers integrated seamlessly. Part of this involved reviewing a
large number of Pull Requests (PRs) over the year but amounted to roughly #PRs review and
closed.

## Key Features

* Upload and parse Moodle-style grading sheets (.csv, .tsv)
* Automatically link student names/emails to Git commit data
* Scale and adjust grades based on contribution metrics
* Download a populated grading file with contribution-weighted scores
* Clean and responsive native UI powered by SvelteKit and Tailwind
* Automatically scale student based on different attributes (e.g. number of commits,
  lines of code (LOC), etc.)
* Fully cross-platform (macOS, Windows, Linux) with rpm, deb, msi and dmg packages.

## Gallery

*Landing screen allowing user to search a repository*
{{ image(src="search.png", alt="Landing screen allowing user to search a repository") }}

*Searching for a repository*
{{ image(src="search-query.png", alt="Searching for a repository (Linux kernel repos at depth 5)") }}

*Loading screen*
{{ image(src="loading-screen.png", alt="Loading screen") }}

*Overview of repository being analysed include mean distribution graph of contributors*
{{ image(src="repo-overview.png", alt="Repository overview screen") }}

*Expanded graph for easier viewing*
{{ image(src="expanded-graph.png", alt="Expanded graph for easier viewing") }}
