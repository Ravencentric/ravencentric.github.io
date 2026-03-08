---
title: "Projects"
menu: "main"
weight: 3
hideReply: true
---

## Projects

Most of my projects start as small tools to solve problems I run into. Some stay small, some grow into libraries.

---

# awesome-arr
_A collection of *arrs and related stuff._ [[GitHub]](https://github.com/Ravencentric/awesome-arr)


Essentially my first project on GitHub, which did not involve any code whatsoever. By the time we get here, I was already deep into
self hosting: Sonarr, Radarr, Plex, etc. I found a cool repository called [`rustyshackleford36/locatarr` (archived snapshot, 2020)](http://web.archive.org/web/20220913033723/https://github.com/rustyshackleford36/locatarr) that collected *arr family of apps. Unfortunately, this repository was deleted at one point, so I decided to make my own. To my surpise, this repository somehow grew to over 3k stars on GitHub.


# juicenet-cli
_CLI tool for uploading files to Usenet._ [[GitHub]](https://github.com/Ravencentric/juicenet-cli) [[PyPI]](https://pypi.org/project/juicenet-cli/) [[Docs]](https://juicenet.ravencentric.cc/)

For all intents and purposes, this was the first piece of code I ever wrote that was more complex than fibonacci.
It started off as a [single file script](https://github.com/Ravencentric/juicenet-cli/tree/ceff3b9e97a173795d2a2b1a8a38052997b20e00) that you coudln't even install from PyPI because I had no idea how to package something. I would be lying if I said I still love this codebase. Best praise I can give this is that it works well but I think I could do way better if I rewrote it from scratch because that's the only way I'll get rid of every architectural mistake I made.

### archivefile
_Unified interface for multiple archive formats_

Working with mixed archives in Python is annoying because tarfile, zipfile, py7zr, and rarfile all behave slightly differently. archivefile wraps them behind a single interface so they can be treated the same way.

### misaki
_Asynchronous link checker written in Rust_

misaki checks large sets of URLs concurrently and optionally integrates with FlareSolverr to deal with Cloudflare protected sites. It is available both as a library and a CLI.

### mkvinfo
_Python library for inspecting Matroska files_

mkvinfo wraps mkvmerge and exposes container metadata and track information as structured Python objects.

### myne
_Parser for manga and light novel filenames_

Release filenames contain metadata like title, volume, year, and release group. myne extracts that information into a structured representation.

---

**NZB related projects**

# nzb
_Python parser and metadata editor for NZB files_

Spec compliant parser designed to read and modify NZB files programmatically.

### nzb-rs
_Rust implementation of the NZB parser_

A Rust port of the parser focused on correctness and performance.

### rnzb
_Python bindings for nzb-rs_

Provides a faster drop-in replacement for the Python parser by using the Rust implementation underneath.

---

**Small API clients**

### pyanilist
_Python wrapper for the AniList API_

Turns AniList responses into structured Python objects.

### pynyaa
_Nyaa release scraper_

Converts nyaa.si releases into Python objects that are easier to work with.

### seadex
_Python wrapper for the SeaDex API_

Client library for accessing curated anime release data.

### privatebin
_Python client for PrivateBin_

Interact with PrivateBin servers to create, retrieve, and delete encrypted pastes.

---

### awesome-arr
_Curated list of *arr ecosystem software_

Collection of tools and resources related to the *arr media automation ecosystem.

### seadex-data-refinement
_Tools for processing SeaDex datasets_

Small scripts and utilities used to refine SeaDex dataset exports.
