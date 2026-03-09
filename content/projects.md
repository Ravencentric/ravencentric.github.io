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

# pyanilist
_Python wrapper for the AniList API_ [[GitHub]](https://github.com/Ravencentric/pyanilist) [[PyPI]](https://pypi.org/project/pyanilist/) [[Docs]](https://ravencentric.cc/pyanilist/)

A type-safe, ergonomic, lazy sync/async wrapper for the AniList. It uses iterators a lot to lazily make requests. I was using the [AniList API](https://docs.anilist.co/) a lot in various scripts to manage my self-hosted collection and existing libraries were pretty much entirely untyped and i wasn't happy with their API.

### pynyaa
_Turn nyaa.si torrent pages into neat Python objects_ [[GitHub]](https://github.com/Ravencentric/pynyaa) [[PyPI]](https://pypi.org/project/pynyaa/) [[Docs]](https://ravencentric.cc/pynyaa/)

Pretty much the same story as above, i.e., type-safe, ergonomic, and lazy sync/async wrapper for the cat site which doesn't offer an API because existing libraries lacked in those departments. This library doesn't download anything, it's just for scraping metadata.

### archivefile
_Unified interface for tar, zip, sevenzip, and rar files_ [[GitHub]](https://github.com/Ravencentric/archivefile) [[PyPI]](https://pypi.org/project/archivefile/) [[Docs]](https://ravencentric.cc/archivefile/)

I was dealing with archive files of various formats and scripting against them quickly became annoying dance of if-else branches to handle the fact that [`tarfile`][tarfile], [`zipfile`][zipfile], [`py7zr`][py7zr], and [`rarfile`][rarfile] all behave differently given the common denominator of tasks. So I set out to fix this and `archivefile` was the result. On a high-level, `ArchieFile` is defined by a protocol, with an API that's covers the common functionality and somewhat inspired by [`pathlib`][pathlib] because I think [`pathlib`][pathlib] is great, which I implement for the aforementioned formats. At runtime, it does a single check to dispatch the correct handler upon initialization and I no longer have to write boilerplate to read a single file from an archive. Also has a fair amount of tests that ensure the handlers produce the same output regardless of the underlying format. 

Developing this also led me to find various bugs and missing functionality in [`py7zr`][py7zr] which I fixed [upstream][py7zr-upstream], becoming the [second highest committer on the project][py7zr-contributors] with [15 or so merged PRs][py7zr-prs].

[tarfile]: https://docs.python.org/3/library/tarfile.html
[zipfile]: https://docs.python.org/3/library/zipfile.html
[py7zr]: https://pypi.org/project/py7zr/
[rarfile]: https://pypi.org/project/rarfile/
[pathlib]: https://docs.python.org/3/library/pathlib.html
[py7zr-upstream]: https://github.com/miurahr/py7zr
[py7zr-contributors]: https://github.com/miurahr/py7zr/graphs/contributors
[py7zr-prs]: https://github.com/miurahr/py7zr/pulls?q=is%3Apr+author%3ARavencentric+is%3Aclosed


# nzb
_A spec compliant parser and meta editor for NZB files._

[[GitHub]](https://github.com/Ravencentric/nzb)
[[PyPI]](https://pypi.org/project/nzb/)
[[Docs]](https://ravencentric.cc/nzb/)

Possibly one of my favorites. It's a performant, pure python, dependency free, type-safe [spec]compliant parser and meta editor for NZB files following the "make invalid states unrepresentable" pattern (well, as long as you stick to the public API because it's Python after all) so if you successfully construct it, you know it's a valid NZB file. Beyond that, it also provides various ergonomic methods for introspecting itself. I've used it  extensively on real world files so i think i can confidently claim it's the best NZB parser in python, however niche that might be.

[spec]: https://sabnzbd.org/wiki/extra/nzb-spec

# seadex
_Python wrapper for the SeaDex API_

[[GitHub]](https://github.com/Ravencentric/seadex)
[[PyPI]](https://pypi.org/project/seadex/)
[[Docs]](https://ravencentric.cc/seadex/)

Pretty much what it says on the tin. It's a fairly standard API wrapper for [SeaDex] following the same ethos as the rest so: type-safe, lazy, efficient.

[SeaDex]: https://releases.moe/about/

# stringenum
_A small, dependency-free library offering additional enum.StrEnum subclasses and a backport for older Python versions._

[[GitHub]](https://github.com/Ravencentric/stringenum)
[[PyPI]](https://pypi.org/project/stringenum/)

This was mostly a toy project because I wanted to try out [metaclasses]. It backports features from latest Python versions down to 3.9 and provides a bunch of other string enums with different properties and guarantees.

[metaclasses]: https://docs.python.org/3/reference/datamodel.html#metaclasses

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
