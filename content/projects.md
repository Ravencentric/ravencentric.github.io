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
_A spec compliant parser and meta editor for NZB files_

[[GitHub]](https://github.com/Ravencentric/nzb)
[[PyPI]](https://pypi.org/project/nzb/)
[[Docs]](https://ravencentric.cc/nzb/)

Possibly one of my favorites. It's a performant, pure python, dependency free, type-safe [spec] compliant parser and meta editor for NZB files following the "make invalid states unrepresentable" pattern (well, as long as you stick to the public API because it's Python after all) so if you successfully construct it, you know it's a valid NZB structure. Beyond that, it also provides various ergonomic methods for introspecting itself. I've used it  extensively on real world files so i think i can confidently claim it's the best NZB parser in python, however niche that might be.

[spec]: https://sabnzbd.org/wiki/extra/nzb-spec

# seadex
_Python wrapper for the SeaDex API_

[[GitHub]](https://github.com/Ravencentric/seadex)
[[PyPI]](https://pypi.org/project/seadex/)
[[Docs]](https://ravencentric.cc/seadex/)

Pretty much what it says on the tin. It's a fairly standard API wrapper for [SeaDex] following the same ethos as the rest so: type-safe, lazy, efficient.

[SeaDex]: https://releases.moe/about/

# stringenum
_A small, dependency-free library offering additional enum.StrEnum subclasses and a backport for older Python versions_

[[GitHub]](https://github.com/Ravencentric/stringenum)
[[PyPI]](https://pypi.org/project/stringenum/)

This was mostly a toy project because I wanted to play with [metaclasses]. It backports features from latest Python versions down to 3.9 and provides a bunch of other string enums with different properties and guarantees.

[metaclasses]: https://docs.python.org/3/reference/datamodel.html#metaclasses


# nzb-rs
_A spec compliant parser for NZB files_

[[GitHub]](https://github.com/Ravencentric/nzb-rs)
[[crates.io]](https://crates.io/crates/nzb-rs)
[[Docs]](https://docs.rs/nzb-rs/latest/nzb_rs/)

Rust had been on my radar for a while. It looked like a fascinating language, so one day I finally sat down and read the [Rust book]. Great tooling (cargo), null safety, a powerful type system, pattern matching - there was a lot to like. Naturally, I needed a project, and parsers seemed like exactly the kind of thing Rust excels at. My [NZB parser] was a perfect candidate: it deals with XML, a format with a long history of security pitfalls that safe Rust largely avoids by design.

The experience turned out to be surprisingly smooth. Moving from Python to Rust felt quite natural, likely thanks to Rust's zero-cost abstractions that feel just as expressive as many Python constructs. Static typing also felt at home since I was already a fan of type hints in Python.

Traits felt like a more powerful blend of dunder methods, `abc.ABC`, and `typing.Protocol`. I miss Rust's enums every time I write Python, and the ergonomic `Option<T>` would make any [PEP-505] fans jealous (myself included).

Despite its reputation, the borrow checker rarely got in my way. The rules felt intuitive: a value has a single owner, and only one mutable reference at a time. I also barely had to think about lifetimes since the compiler inferred most of them. Things only became rougher when I experimented with async and started running into more cryptic borrow-checker errors, but that is a story for another time.

With those impressions in mind, the project itself evolved alongside my understanding of Rust. The [first version] of the parser was a fairly direct port of the [Python implementation] with an almost identical API. Even so, the Rust version ended up being several times faster. Some of that came from Rust itself, but the process also highlighted inefficiencies in the Python implementation. After taking those lessons back, I was able to remove quite a bit of slower code and narrow the performance gap. Later versions of the Rust library evolved into a more idiomatic API. Ironically, the Python implementation eventually [adopted] that API as well.

[Rust book]: https://doc.rust-lang.org/book/
[NZB parser]: https://github.com/Ravencentric/nzb
[PEP-505]: https://peps.python.org/pep-0505/
[first version]: https://docs.rs/nzb-rs/0.1.0/nzb_rs/
[Python implementation]: https://github.com/Ravencentric/nzb/blob/v0.3.0/src/nzb/_core.py
[adopted]: https://github.com/Ravencentric/nzb/releases/tag/v0.4.0

# rnzb
_Python bindings to the nzb-rs library - a spec compliant parser for NZB files, written in Rust_

[[GitHub]](https://github.com/Ravencentric/rnzb)
[[PyPI]](https://pypi.org/project/rnzb/)

A third NZB parser? Yes. At this point I might have a problem.

After writing one in Python and another in Rust, I had a thought: "Wouldn't it be cool to use the Rust implementation directly from Python and get the best of both worlds?" Spoiler alert: yes.

My goal was simple: a drop-in replacement so existing Python code could immediately benefit from the Rust parser.

After a bit of research I found [PyO3], which powers Pydantic and many other Rust-powered Python extensions. There were a few small hiccups along the way, but the PyO3 docs and maintainers were incredibly helpful whenever I had questions.

Once everything worked and the tests passed, the next step was packaging. Wheels are prebuilt Python packages so users do not have to compile anything during installation. To build wheels for multiple platforms I used [pypa/cibuildwheel], which seems to be what most projects rely on for this.

One interesting detail about extension wheels is Python version compatibility. For example: `rnzb-0.6.0-cp314-cp314-win_arm64.whl`. The `cp314-cp314` tag means the wheel only works on CPython 3.14. On newer versions the installer falls back to the source distribution (sdist) and tries to build it locally, which usually fails unless a Rust toolchain is installed.

To avoid releasing new wheels for every Python version, I built the extension against the [Limited C API] (abi3), which PyO3 supports. The result looks like this: `rnzb-0.6.0-cp39-abi3-win_amd64.whl`. The `cp39-abi3` tag means the same wheel works on every CPython version starting from Python 3.9.

So yes, the end result was a third NZB parser. But this one is a little different: rnzb brings the speed and safety of the Rust implementation to Python while keeping a familiar drop-in API. Python users get the fast Rust parser without changing their code.

[PyO3]: https://pyo3.rs/
[pypa/cibuildwheel]: https://github.com/pypa/cibuildwheel
[Limited C API]: https://docs.python.org/3/c-api/stable.html#limited-c-api

# atomicwriter
_Cross-platform atomic file writer for all-or-nothing operations._

[[GitHub]](https://github.com/Ravencentric/atomicwriter)
[[PyPI]](https://pypi.org/project/atomicwriter/)
[[Docs]](https://ravencentric.cc/atomicwriter/)

Every now and then I run into situations where I need to write something atomically. There used to be a package for this, [`python-atomicwrites`], but it is unmaintained now.

I am by no means an expert in file system behavior across different platforms, but I knew of a library that was: Rust's [`tempfile`] crate. It already implements atomic writes (and a lot more) correctly across platforms.

Thanks to everything I learned from my previous projects, this one was fairly straightforward. I wrote a thin wrapper around the Rust library, exposed an idiomatic Python API, added some tests, and published it with abi3 wheels.

[`python-atomicwrites`]: https://github.com/untitaker/python-atomicwrites
[`tempfile`]: https://docs.rs/tempfile/latest/tempfile/


# privatebin
_Python library for interacting with PrivateBin's v2 API (PrivateBin >= 1.3) to create, retrieve, and delete encrypted pastes._

[[GitHub]](https://github.com/Ravencentric/privatebin)
[[PyPI]](https://pypi.org/project/privatebin/)
[[Docs]](https://ravencentric.cc/privatebin/)

I regularly use an instance of [PrivateBin] to temporarily and securely store logs from various scripts. Initially I used the [PrivateBinAPI] package. It had incomplete type hints and I was not a fan of the API, but it worked... until it [didn't].

So I set out to write one from scratch.

PrivateBin never actually sees your paste. Everything is encrypted client-side before being sent to the server, which only stores and returns the encrypted blob. The client therefore has to handle both encryption and decryption.

Unfortunately (and maybe this was just me being dumb), the PrivateBin documentation did not help much here. So I started digging through the source code of [PrivateBinAPI] to figure out how it handled the protocol and encryption.

Turns out... it doesn't. It depends on another PrivateBin library, [PBinCLI], to actually do the talking to PrivateBin.

So I went and read that code instead. After a few hours of trial, error, and carefully extracting pieces of logic, I eventually isolated the core encryption and decryption routines. Once that part was figured out, implementing the rest of the client was fairly straightforward.

The result is a small, typed PrivateBin client with a clean API and no dependency on another PrivateBin library.

[PrivateBin]: https://privatebin.info/
[PrivateBinAPI]: https://github.com/Pioverpie/privatebin-api/
[didn't]: https://github.com/Pioverpie/privatebin-api/issues/12
[PBinCLI]: https://github.com/r4sas/PBinCLI

# myne
_Parser for manga and light novel filenames_

[[GitHub]](https://github.com/Ravencentric/myne)
[[PyPI]](https://pypi.org/project/myne/)
[[Docs]](https://ravencentric.cc/myne/)

I was surprised I couldn't find a library that parsed manga and light novel filenames into structured metadata like title, volume, and chapter.

It's written in Rust with simple Python bindings. Under the hood it is entirely regex-based, and if you peek inside you will find some pretty gnarly expressions. Manga and light novels do not follow any real naming standard, so everything is based on convention. You would be surprised how terrible some of these filenames can get.

The name "myne" comes from the main character of [Ascendance of a Bookworm], who really loves books.
I'm still pretty proud of that one.

Despite that, myne has parsed every real-world filename I have thrown at it so far.

[Ascendance of a Bookworm]: https://j-novel.club/series/ascendance-of-a-bookworm

# mkvinfo
_Python library for probing matroska files with mkvmerge_

[[GitHub]](https://github.com/Ravencentric/mkvinfo)
[[PyPI]](https://pypi.org/project/mkvinfo/)
[[Docs]](https://ravencentric.cc/mkvinfo/)

I needed a way to introspect MKV files so I could classify them further. [`mkvmerge`] already exposes a lot of useful metadata, but consuming that output directly from Python gets annoying pretty quickly.

So this library is basically a thin wrapper around that. It runs `mkvmerge -J` and turns the JSON output into typed Python objects so you can easily inspect tracks, codecs, and other metadata.

[`mkvmerge`]: https://mkvtoolnix.download/doc/mkvmerge.html

# misaki
_misaki is a fast, asynchronous link checker with optional FlareSolverr support._

[[GitHub]](https://github.com/Ravencentric/misaki)
[[crates.io: misaki-core]](https://crates.io/crates/misaki-core)
[[crates.io: misaki-cli]](https://crates.io/crates/misaki-cli)
[[Docs]](https://docs.rs/misaki_core/latest/misaki_core/)

A good friend of mine wanted a link checker with flaresolverr support, so I wrote one.
Now, a link checker is a pretty easy project and it was a perfect excuse to try out
async Rust and this was the first time Rust felt like a pain. The errors got quite a bit cryptic and
it felt like async support is still somewhat incomplete.

Some patterns that should be simple end up awkward. For example, there is no built-in way to “yield” values from async code, so you end up relying on third-party crates like [`async-stream`] to emulate async generators. There is also no async drop, which makes cleaning up async resources harder than it should be.

[`async-stream`]: https://docs.rs/async-stream/latest/async_stream/