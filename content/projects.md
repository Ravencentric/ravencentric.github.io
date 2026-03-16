---
title: "Projects"
menu: "main"
weight: 3
hideReply: true
---

Most of my projects start as small tools to solve problems I run into. Some stay small,
some grow into libraries.

---

## awesome-arr

*A collection of *arrs and related stuff*

[[GitHub]](https://GitHub.com/Ravencentric/awesome-arr)

Essentially my first project on GitHub, which didn’t involve any code whatsoever.

By the time we get here, I was already deep into self-hosting: Sonarr, Radarr, Plex, etc. During this time, I came across a cool repository, [rustyshackleford36/locatarr], that collected *arr-family apps. I used to check it every now and then, but at some point the repository disappeared, so I decided to make my own while also greatly expanding on the original collection. To my surprise, it somehow ended up with over 3k stars on GitHub.

[rustyshackleford36/locatarr]: http://web.archive.org/web/20220913033723/https://github.com/rustyshackleford36/locatarr

## juicenet-cli

*CLI tool designed to simplify the process of uploading files to Usenet*

[[GitHub]](https://GitHub.com/Ravencentric/juicenet-cli)
[[PyPI]](https://pypi.org/project/juicenet-cli/)
[[Docs]](https://juicenet.ravencentric.cc/)

For all intents and purposes, this was the first piece of code I ever wrote that was
more complex than Fibonacci. It started off as a [single-file script] that you couldn't even install from PyPI because I had no idea how to package anything.

I would be lying if I said I still love this codebase. The best praise I can give it is
that it works well and is likely one of the few (if not the only) uploaders that preserves
folders without using [RAR], relying on PAR2 files instead (if you don't know much about
Usenet, it has no concept of folders).

But I think I could do way better if I rewrote it from scratch, because that's probably
the only way I'd get rid of every architectural mistake I made. The problem is that doing
so would likely break a fair number of users the project has gained, so it's a tough
choice.

[single-file script]: https://github.com/Ravencentric/juicenet-cli/tree/ceff3b9e97a173795d2a2b1a8a38052997b20e00
[RAR]: https://github.com/animetosho/Nyuu/wiki/Stop-RAR-Uploads



## pyanilist

*Python wrapper for the AniList API*

[[GitHub]](https://GitHub.com/Ravencentric/pyanilist)
[[PyPI]](https://pypi.org/project/pyanilist/)
[[Docs]](https://ravencentric.cc/pyanilist/)

I use the [AniList API] in a lot of scripts to manage my self-hosted collection. For a while I stuck with existing libraries, but issues kept cropping up and I was never really happy with them - most lacked proper type hints, structured objects, or both. Some also seemed entirely unmaintained. After enough `TypeError`s and expressions like `data["Media"][0]["title"]["english"]`, I finally gave up and started writing my own.

How hard can an API wrapper be anyway? Just parse the API and be done with it. Except AniList is GraphQL, with a pretty large surface area, circular relationships, and some awkward response shapes. That probably explains why nobody seemed eager to maintain an AniList library. After thinking about it for a bit, I decided I would rather have an ergonomic API than try to cover everything AniList exposes, so I narrowed things down to the parts I actually needed and focused on making those simple.

I also ended up post-processing responses because AniList is not particularly consistent. Sometimes "empty" nested objects have every field set to null, sometimes the entire object is just null, and arrays occasionally contain null elements for good measure. The wrapper normalizes these cases so the returned values match the type hints and require fewer None checks.

On a slight tangent, this is also a project where I really wish Python had some form of [`None`-aware operator].

[AniList API]: https://docs.anilist.co/
[`None`-aware operator]: https://peps.python.org/pep-0505/

## pynyaa

*Turn nyaa.si torrent pages into neat Python objects*

[[GitHub]](https://GitHub.com/Ravencentric/pynyaa)
[[PyPI]](https://pypi.org/project/pynyaa/)
[[Docs]](https://ravencentric.cc/pynyaa/)

AniList metadata wasn't the only thing my scripts needed, but unfortunately
Nyaa does not offer any API. I looked around for existing libraries but didn't
find anything satisfactory.

So I wrote a small function that parsed the page for the few fields I needed.
That worked for a while, but the function kept growing as I needed more and
more metadata from Nyaa. Eventually it got messy enough that I decided to turn
it into a standalone library.

The first release ended up with about ten dependencies (seven if you were on
Python >= 3.11): things like pydantic, lxml, hishel for caching, and a few
others. This wasn't really an issue for me at the time, but as I worked on more
projects I developed a stronger stance against unnecessary dependencies and
libraries that try to do too much.

When I came back to this project, that was something I wanted to fix. For
example, caching is better handled by passing a custom HTTP client configured
however you like. And since the library already validates everything manually,
pydantic ended up being little more than a fancy dataclass.

Later versions gradually removed most of these dependencies while also
simplifying the codebase. These days it only depends on two packages regardless
of Python version: a network stack (httpx) and an HTML parser (beautifulsoup4).

At this point it covers pretty much every field Nyaa exposes and has completely
replaced my original scraping code. It returns type-safe, structured objects
and works in both sync and async code, and since I rely on it heavily in my own
scripts it has also ended up fairly battle-tested against real-world cases.

### archivefile

*Unified interface for tar, zip, sevenzip, and rar files*

[[GitHub]](https://GitHub.com/Ravencentric/archivefile)
[[PyPI]](https://pypi.org/project/archivefile/)
[[Docs]](https://ravencentric.cc/archivefile/)

I was dealing with archive files of various formats and scripting against them quickly
became annoying dance of if-else branches to handle the fact that [`tarfile`][tarfile],
[`zipfile`][zipfile], [`py7zr`][py7zr], and [`rarfile`][rarfile] all behave differently
given the common denominator of tasks. So I set out to fix this and `archivefile` was
the result. On a high-level, `ArchieFile` is defined by a protocol, with an API that's
covers the common functionality and somewhat inspired by [`pathlib`][pathlib] because I
think [`pathlib`][pathlib] is great, which I implement for the aforementioned formats.
At runtime, it does a single check to dispatch the correct handler upon initialization
and I no longer have to write boilerplate to read a single file from an archive. Also
has a fair amount of tests that ensure the handlers produce the same output regardless
of the underlying format.

Developing this also led me to find various bugs and missing functionality in
[`py7zr`][py7zr] which I fixed [upstream][py7zr-upstream], becoming the
[second highest committer on the project][py7zr-contributors] with
[15 or so merged PRs][py7zr-prs].

[tarfile]: https://docs.python.org/3/library/tarfile.html
[zipfile]: https://docs.python.org/3/library/zipfile.html
[py7zr]: https://pypi.org/project/py7zr/
[rarfile]: https://pypi.org/project/rarfile/
[pathlib]: https://docs.python.org/3/library/pathlib.html
[py7zr-upstream]: https://github.com/miurahr/py7zr
[py7zr-contributors]: https://github.com/miurahr/py7zr/graphs/contributors
[py7zr-prs]: https://github.com/miurahr/py7zr/pulls?q=is%3Apr+author%3ARavencentric+is%3Aclosed

## NZB

*A spec compliant parser and meta editor for NZB files*

[[GitHub]](https://GitHub.com/Ravencentric/NZB)
[[PyPI]](https://pypi.org/project/NZB/)
[[Docs]](https://ravencentric.cc/NZB/)

Possibly one of my favorites. It's a performant, pure Python, dependency free, type-safe
[spec] compliant parser and meta editor for NZB files following the "make invalid states
unrepresentable" pattern (well, as long as you stick to the public API because it's
Python after all) so if you successfully construct it, you know it's a valid NZB
structure. Beyond that, it also provides various ergonomic methods for introspecting
itself. I've used it extensively on real world files so i think i can confidently claim
it's the best NZB parser in Python, however niche that might be.

[spec]: https://sabnzbd.org/wiki/extra/nzb-spec

## seadex

*Python wrapper for the SeaDex API*

[[GitHub]](https://GitHub.com/Ravencentric/seadex)
[[PyPI]](https://pypi.org/project/seadex/)
[[Docs]](https://ravencentric.cc/seadex/)

Pretty much what it says on the tin. It's a fairly standard API wrapper for [SeaDex]
following the same ethos as the rest so: type-safe, lazy, efficient.

[SeaDex]: https://releases.moe/about/

## stringenum

_A small, dependency-free library offering additional enum.StrEnum subclasses and a
backport for older Python versions_

[[GitHub]](https://GitHub.com/Ravencentric/stringenum)
[[PyPI]](https://pypi.org/project/stringenum/)

This was mostly a toy project because I wanted to play with [metaclasses]. It backports
features from latest Python versions down to 3.9 and provides a bunch of other string
enums with different properties and guarantees.

[metaclasses]: https://docs.python.org/3/reference/datamodel.html#metaclasses

## NZB-rs

*A spec compliant parser for NZB files*

[[GitHub]](https://GitHub.com/Ravencentric/NZB-rs)
[[crates.io]](https://crates.io/crates/NZB-rs)
[[Docs]](https://docs.rs/NZB-rs/latest/NZB_rs/)

Rust had been on my radar for a while. It looked like a fascinating language, so one day
I finally sat down and read the [Rust book]. Great tooling (cargo), null safety, a
powerful type system, pattern matching - there was a lot to like. Naturally, I needed a
project, and parsers seemed like exactly the kind of thing Rust excels at. My
[NZB parser] was a perfect candidate: it deals with XML, a format with a long history of
security pitfalls that safe Rust largely avoids by design.

The experience turned out to be surprisingly smooth. Moving from Python to Rust felt
quite natural, likely thanks to Rust's zero-cost abstractions that feel just as
expressive as many Python constructs. Static typing also felt at home since I was
already a fan of type hints in Python.

Traits felt like a more powerful blend of dunder methods, `abc.ABC`, and
`typing.Protocol`. I miss Rust's enums every time I write Python, and the ergonomic
`Option<T>` would make any [PEP-505] fans jealous (myself included).

Despite its reputation, the borrow checker rarely got in my way. The rules felt
intuitive: a value has a single owner, and only one mutable reference at a time. I also
barely had to think about lifetimes since the compiler inferred most of them. Things
only became rougher when I experimented with async and started running into more cryptic
borrow-checker errors, but that is a story for another time.

With those impressions in mind, the project itself evolved alongside my understanding of
Rust. The [first version] of the parser was a fairly direct port of the
[Python implementation] with an almost identical API. Even so, the Rust version ended up
being several times faster. Some of that came from Rust itself, but the process also
highlighted inefficiencies in the Python implementation. After taking those lessons
back, I was able to remove quite a bit of slower code and narrow the performance gap.
Later versions of the Rust library evolved into a more idiomatic API. Ironically, the
Python implementation eventually [adopted] that API as well.

[Rust book]: https://doc.rust-lang.org/book/
[NZB parser]: https://github.com/Ravencentric/nzb
[PEP-505]: https://peps.python.org/pep-0505/
[first version]: https://docs.rs/nzb-rs/0.1.0/nzb_rs/
[Python implementation]: https://github.com/Ravencentric/nzb/blob/v0.3.0/src/nzb/_core.py
[adopted]: https://github.com/Ravencentric/nzb/releases/tag/v0.4.0

## rnzb

_Python bindings to the NZB-rs library - a spec compliant parser for NZB files, written
in Rust_

[[GitHub]](https://GitHub.com/Ravencentric/rnzb)
[[PyPI]](https://pypi.org/project/rnzb/)

A third NZB parser? Yes. At this point I might have a problem.

After writing one in Python and another in Rust, I had a thought: "Wouldn't it be cool
to use the Rust implementation directly from Python and get the best of both worlds?"
Spoiler alert: yes.

My goal was simple: a drop-in replacement so existing Python code could immediately
benefit from the Rust parser.

After a bit of research I found [PyO3], which powers Pydantic and many other
Rust-powered Python extensions. There were a few small hiccups along the way, but the
PyO3 docs and maintainers were incredibly helpful whenever I had questions.

Once everything worked and the tests passed, the next step was packaging. Wheels are
prebuilt Python packages so users do not have to compile anything during installation.
To build wheels for multiple platforms I used [pypa/cibuildwheel], which seems to be
what most projects rely on for this.

One interesting detail about extension wheels is Python version compatibility. For
example: `rnzb-0.6.0-cp314-cp314-win_arm64.whl`. The `cp314-cp314` tag means the wheel
only works on CPython 3.14. On newer versions the installer falls back to the source
distribution (sdist) and tries to build it locally, which usually fails unless a Rust
toolchain is installed.

To avoid releasing new wheels for every Python version, I built the extension against
the [Limited C API] (abi3), which PyO3 supports. The result looks like this:
`rnzb-0.6.0-cp39-abi3-win_amd64.whl`. The `cp39-abi3` tag means the same wheel works on
every CPython version starting from Python 3.9.

So yes, the end result was a third NZB parser. But this one is a little different: rnzb
brings the speed and safety of the Rust implementation to Python while keeping a
familiar drop-in API. Python users get the fast Rust parser without changing their code.

[PyO3]: https://pyo3.rs/
[pypa/cibuildwheel]: https://github.com/pypa/cibuildwheel
[Limited C API]: https://docs.python.org/3/c-api/stable.html#limited-c-api

## atomicwriter

*Cross-platform atomic file writer for all-or-nothing operations.*

[[GitHub]](https://GitHub.com/Ravencentric/atomicwriter)
[[PyPI]](https://pypi.org/project/atomicwriter/)
[[Docs]](https://ravencentric.cc/atomicwriter/)

Every now and then I run into situations where I need to write something atomically.
There used to be a package for this, [`python-atomicwrites`], but it is unmaintained
now.

I am by no means an expert in file system behavior across different platforms, but I
knew of a library that was: Rust's [`tempfile`] crate. It already implements atomic
writes (and a lot more) correctly across platforms.

Thanks to everything I learned from my previous projects, this one was fairly
straightforward. I wrote a thin wrapper around the Rust library, exposed an idiomatic
Python API, added some tests, and published it with abi3 wheels.

[`python-atomicwrites`]: https://github.com/untitaker/python-atomicwrites
[`tempfile`]: https://docs.rs/tempfile/latest/tempfile/

## PrivateBin

_Python library for interacting with PrivateBin's v2 API (PrivateBin >= 1.3) to create,
retrieve, and delete encrypted pastes._

[[GitHub]](https://GitHub.com/Ravencentric/PrivateBin)
[[PyPI]](https://pypi.org/project/PrivateBin/)
[[Docs]](https://ravencentric.cc/PrivateBin/)

I regularly use an instance of [PrivateBin] to temporarily and securely store logs from
various scripts. Initially I used the [PrivateBinAPI] package. It had incomplete type
hints and I was not a fan of the API, but it worked... until it [didn't].

So I set out to write one from scratch.

PrivateBin never actually sees your paste. Everything is encrypted client-side before
being sent to the server, which only stores and returns the encrypted blob. The client
therefore has to handle both encryption and decryption.

Unfortunately (and maybe this was just me being dumb), the PrivateBin documentation did
not help much here. So I started digging through the source code of [PrivateBinAPI] to
figure out how it handled the protocol and encryption.

Turns out... it doesn't. It depends on another PrivateBin library, [PBinCLI], to
actually do the talking to PrivateBin.

So I went and read that code instead. After a few hours of trial, error, and carefully
extracting pieces of logic, I eventually isolated the core encryption and decryption
routines. Once that part was figured out, implementing the rest of the client was fairly
straightforward.

The result is a small, typed PrivateBin client with a clean API and no dependency on
another PrivateBin library.

[PrivateBin]: https://privatebin.info/
[PrivateBinAPI]: https://github.com/Pioverpie/privatebin-api/
[didn't]: https://github.com/Pioverpie/privatebin-api/issues/12
[PBinCLI]: https://github.com/r4sas/PBinCLI

## myne

*Parser for manga and light novel filenames*

[[GitHub]](https://GitHub.com/Ravencentric/myne)
[[PyPI]](https://pypi.org/project/myne/)
[[Docs]](https://ravencentric.cc/myne/)

I was surprised I couldn't find a library that parsed manga and light novel filenames
into structured metadata like title, volume, and chapter.

It's written in Rust with simple Python bindings. Under the hood it is entirely
regex-based, and if you peek inside you will find some pretty gnarly expressions. Manga
and light novels do not follow any real naming standard, so everything is based on
convention. You would be surprised how terrible some of these filenames can get.

The name "myne" comes from the main character of [Ascendance of a Bookworm], who really
loves books. I'm still pretty proud of that one.

Despite that, myne has parsed every real-world filename I have thrown at it so far.

[Ascendance of a Bookworm]: https://j-novel.club/series/ascendance-of-a-bookworm

## mkvinfo

*Python library for probing matroska files with mkvmerge*

[[GitHub]](https://GitHub.com/Ravencentric/mkvinfo)
[[PyPI]](https://pypi.org/project/mkvinfo/)
[[Docs]](https://ravencentric.cc/mkvinfo/)

I needed a way to introspect MKV files so I could classify them further. [`mkvmerge`]
already exposes a lot of useful metadata, but consuming that output directly from Python
gets annoying pretty quickly.

So this library is basically a thin wrapper around that. It runs `mkvmerge -J` and turns
the JSON output into typed Python objects so you can easily inspect tracks, codecs, and
other metadata.

[`mkvmerge`]: https://mkvtoolnix.download/doc/mkvmerge.html

## misaki

*misaki is a fast, asynchronous link checker with optional FlareSolverr support.*

[[GitHub]](https://GitHub.com/Ravencentric/misaki)
[[crates.io: misaki-core]](https://crates.io/crates/misaki-core)
[[crates.io: misaki-cli]](https://crates.io/crates/misaki-cli)
[[Docs]](https://docs.rs/misaki_core/latest/misaki_core/)

A good friend of mine wanted a link checker with flaresolverr support, so I wrote one.
Now, a link checker is a pretty easy project and it was a perfect excuse to try out
async Rust and this was the first time Rust felt like a pain. The errors got quite a bit
cryptic and it felt like async support is still somewhat incomplete.

Some patterns that should be simple end up awkward. For example, there is no built-in
way to “yield” values from async code, so you end up relying on third-party crates like
[`async-stream`] to emulate async generators. There is also no async drop, which makes
cleaning up async resources harder than it should be.

[`async-stream`]: https://docs.rs/async-stream/latest/async_stream/
