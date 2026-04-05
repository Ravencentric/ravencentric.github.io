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

By the time we get here, I was already deep into self-hosting: Sonarr, Radarr, Plex,
etc. During this time, I came across a cool repository, [rustyshackleford36/locatarr],
that collected *arr-family apps. I used to check it every now and then, but at some
point the repository disappeared, so I decided to make my own while also greatly
expanding on the original collection. To my surprise, it somehow ended up with over 3k
stars on GitHub.

[rustyshackleford36/locatarr]: http://web.archive.org/web/20220913033723/https://github.com/rustyshackleford36/locatarr

## juicenet-cli

*CLI tool designed to simplify the process of uploading files to Usenet*

[[GitHub]](https://GitHub.com/Ravencentric/juicenet-cli)
[[PyPI]](https://pypi.org/project/juicenet-cli/)
[[Docs]](https://juicenet.ravencentric.cc/)

For all intents and purposes, this was the first piece of code I ever wrote that was
more complex than Fibonacci. It started off as a [single-file script] that you couldn't
even install from PyPI because I had no idea how to package anything.

If you don't know much about uploading things to usenet, you can't upload folders, only
individual files, and you should obfuscate them. Typical workflow is that your uploader
splits the file into pieces called "articles" (if you're familiar with torrenting, this
is similar), uploads them, and records them in an [NZB] file. Downloaders then use the
NZB to grab each article and reconstruct the file. If a single article is lost, deleted,
or corrupted, everything breaks, so there are also parity files called [PAR2] which are
generated for a given data file and uploaded together, allowing the client to repair the
file up to a certain threshold.

Now, pretty much all existing Usenet uploaders wrap your file or folder in split,
obfuscated RAR archives which then get split into articles. They then write this
obfuscated nonsense in the NZB file, which means you can no longer statically parse it
to get any metadata. And obfuscating the NZB is worse than useless. You only really
wanna obfuscate the data you're uploading, because once you have the NZB you can
download the file regardless. The RAR step may have served some purpose 20 years ago but
it's entirely wasteful now. It's probably a mix of history, people following existing
practices blindly, and the misconception that RARing is necessary for obfuscation and/or
preserving a folder.

And you know what else PAR2 files can do? They can rename the file to its original name,
which you can use to reconstruct the folder structure. So we no longer need RARs for
that. RAR is also unnecessary for obfuscation, since that already happens at the article
level. While testing this setup, I also found a [bug] in SABnzbd where it failed to
correctly reconstruct the folder structure from PAR2 alone, which got fixed pretty
quickly.

Now, I'm not the first one to notice any of this. Everything I've said here I learned
from [@animetosho]'s extremely well written article, [Stop RAR Uploads], which goes into
more detail. They also happen to maintain the best Usenet tooling there is: [Nyuu] as
the uploader, and [ParPar] as the PAR2 generator. Nyuu
[doesn't generate PAR2 files by default][#58] and ParPar doesn't preserve anything but
the basename by default, which makes sense, as they can't really assume what the user
wants.

But I can.

So the next step was familiarizing myself with Nyuu and ParPar. Mostly ParPar, since I
had to figure out how to preserve folders with ParPar's [`--filepath-format`] option.
After that, I wrote a tiny script that calls ParPar and Nyuu with the correct arguments
and called it Juicenet.

The quality of the code in Juicenet hasn't aged well. It's very much a novice's first
attempt and it shows.

Still, it has gained more users than I ever expected, likely because to this day it's
one of the few, if not the only, high level uploaders that does everything without using
RAR archives. I can definitely do way better if I rewrote it from scratch, because
that's probably the only way I'd get rid of every architectural mistake I made, but the
fact that this has real users means I'll end up breaking them, so it's not exactly an
easy choice.

[single-file script]: https://github.com/Ravencentric/juicenet-cli/tree/ceff3b9e97a173795d2a2b1a8a38052997b20e00
[NZB]: https://en.wikipedia.org/wiki/NZB
[PAR2]: https://en.wikipedia.org/wiki/Parchive#Par2
[bug]: https://github.com/sabnzbd/sabnzbd/issues/2626
[@animetosho]: https://github.com/animetosho
[Stop RAR Uploads]: https://github.com/animetosho/Nyuu/wiki/Stop-RAR-Uploads
[Nyuu]: https://github.com/animetosho/Nyuu
[ParPar]: https://github.com/animetosho/ParPar
[#58]: https://github.com/animetosho/Nyuu/issues/5
[`--filepath-format`]: https://juicenet.ravencentric.cc/archive/parpar-filepath-formats/

## pyanilist

*Python wrapper for the AniList API*

[[GitHub]](https://GitHub.com/Ravencentric/pyanilist)
[[PyPI]](https://pypi.org/project/pyanilist/)
[[Docs]](https://ravencentric.cc/pyanilist/)

I use the [AniList API] in a lot of scripts to manage my self-hosted collection. For a
while I stuck with existing libraries, but issues kept cropping up and I was never
really happy with them - most lacked proper type hints, structured objects, or both.
Some also seemed entirely unmaintained. After enough `TypeError`s and expressions like
`data["Media"][0]["title"]["english"]`, I finally gave up and started writing my own.

How hard can an API wrapper be anyway? Just parse the API and be done with it. Except
AniList is GraphQL, with a pretty large surface area, circular relationships, and some
awkward response shapes. That probably explains why nobody seemed eager to maintain an
AniList library. After thinking about it, I decided I would rather have an ergonomic API
than try to cover everything AniList exposes, so I narrowed things down to what I
actually needed and focused on making that simple.

I also ended up post-processing responses because AniList is not particularly
consistent. Sometimes "empty" nested objects have every field set to null, sometimes the
entire object is just null, and arrays occasionally contain null elements. The wrapper
normalizes these cases so the returned values match the type hints and require fewer
None checks.

The first version of this library released with 9 required dependencies. This wasn't a
problem for me at the time, but as I worked on more projects I started developing a
stronger stance on unnecessary dependencies. For example, a dependency that's slow to
update blocks my entire project from updating to the next Python version. So as I worked
on this further, I slowly removed most of them, to the point where the latest version
only depends on two things: a networking stack (httpx) and a data validation library
(msgspec).

On a slight tangent, this is also a project where I really wish Python had some form of
[`None`-aware operator].

[AniList API]: https://docs.anilist.co/
[`None`-aware operator]: https://peps.python.org/pep-0505/

## pynyaa

*Turn nyaa.si torrent pages into neat Python objects*

[[GitHub]](https://GitHub.com/Ravencentric/pynyaa)
[[PyPI]](https://pypi.org/project/pynyaa/)
[[Docs]](https://ravencentric.cc/pynyaa/)

AniList metadata wasn't the only thing my scripts needed, but Nyaa does not offer any
API. I looked around for existing libraries but didn't find anything satisfactory.

So I wrote a small function that parsed the page for the few fields I needed. That
worked for a while, but it kept growing as I needed more metadata from Nyaa. At some
point it started hitting quirks of how the site represents things (a release can be both
trusted and a remake, but since the panel only has a single color it ends up red), and
eventually it got messy enough that I decided to turn it into a standalone library.

The first release tried to do too much and ended up with about ten dependencies. It
handled caching, parsed torrent files, used pydantic despite already validating
everything by hand, and pulled in lxml when the standard library would have been enough.

That made it harder to use in different contexts, since it forced those decisions onto
the user, so I started stripping those pieces out. These days it's a lightweight library
that just focuses on parsing Nyaa pages, and only depends on two packages: a network
stack (httpx) and an HTML parser (beautifulsoup4).

At this point it covers pretty much every field Nyaa exposes and has completely replaced
my original scraping code. It returns type-safe, structured objects and works in both
sync and async code. Since I rely on it heavily in my own scripts, it has also ended up
fairly battle-tested against real-world cases.

### archivefile

*Unified interface for tar, zip, sevenzip, and rar files*

[[GitHub]](https://GitHub.com/Ravencentric/archivefile)
[[PyPI]](https://pypi.org/project/archivefile/)
[[Docs]](https://ravencentric.cc/archivefile/)

I was dealing with archive files of various formats and scripting against them quickly
became an annoying dance of if-else branches to handle the fact that [tarfile][tarfile],
[zipfile][zipfile], [py7zr][py7zr], and [rarfile][rarfile] all behave differently
despite doing the same basic things. So I wrote `archivefile`.

On a high level, the `ArchiveFile` class is defined by a protocol with an API that
covers the common functionality. It’s somewhat inspired by [pathlib][pathlib], which I
think is great. I then implement this for the aforementioned formats. At runtime, it
does a single check to dispatch the correct handler, and I no longer have to write
boilerplate just to read a single file from an archive.

In fact, due to this approach, every method under `ArchiveFile` has a single line worth
of body:

```py
    def get_member(self, member: StrPath | ArchiveMember) -> ArchiveMember:
        """
        <Docstring redacted for brevity>
        """
        return self._adapter.get_member(member)
```

It also has a fair amount of tests to ensure the handlers produce the same output
regardless of the underlying format.

Developing this led me to find various bugs and missing functionality in [py7zr][py7zr],
which I fixed [upstream][py7zr-upstream], becoming the
[second highest committer on the project][py7zr-contributors] with
[15 or so merged PRs][py7zr-prs]. Thanks to the py7zr maintainer, [@miurahr], for being
great to work with and accepting my PRs.

[tarfile]: https://docs.python.org/3/library/tarfile.html
[zipfile]: https://docs.python.org/3/library/zipfile.html
[py7zr]: https://pypi.org/project/py7zr/
[rarfile]: https://pypi.org/project/rarfile/
[pathlib]: https://docs.python.org/3/library/pathlib.html
[py7zr-upstream]: https://github.com/miurahr/py7zr
[py7zr-contributors]: https://github.com/miurahr/py7zr/graphs/contributors
[py7zr-prs]: https://github.com/miurahr/py7zr/pulls?q=is%3Apr+author%3ARavencentric+is%3Aclosed
[@miurahr]: https://github.com/miurahr

## nzb

*A spec compliant parser and meta editor for NZB files*

[[GitHub]](https://GitHub.com/Ravencentric/NZB)
[[PyPI]](https://pypi.org/project/NZB/)
[[Docs]](https://ravencentric.cc/NZB/)

Possibly one of my favorites. It's a performant, pure Python, dependency free, type-safe
[spec] compliant parser and meta editor for NZB files following the "make invalid states
unrepresentable" pattern (well, as long as you stick to the public API, because it's
Python after all), so if you successfully construct it, you know it's a valid NZB
structure.

Before working on this, I was convinced XML is a scary format and even added a
dependency, [`xmltodict`], to avoid dealing with it, but as you can imagine the
resulting dict isn't pleasant to work with. XML just doesn't lend itself well to that
kind of transformation. After working on the Rust implementation (spoilers), which
forced me to deal with XML because there wasn't an equivalent to `xmltodict`, I realized
XML isn't scary at all. So I dropped `xmltodict` here as well and switched to the
stdlib's [`xml.etree.ElementTree`], which is significantly faster and easier to work
with.

Beyond that, it also provides various ergonomic methods for introspecting itself. I've
used it extensively on real world files, so I think I can confidently claim it's the
best NZB parser in Python, however niche that might be.

[spec]: https://sabnzbd.org/wiki/extra/nzb-spec
[`xmltodict`]: https://pypi.org/project/xmltodict/
[`xml.etree.ElementTree`]: https://docs.python.org/3/library/xml.etree.elementtree.html

## seadex

*Python wrapper for the SeaDex API*

[[GitHub]](https://GitHub.com/Ravencentric/seadex)
[[PyPI]](https://pypi.org/project/seadex/)
[[Docs]](https://ravencentric.cc/seadex/)

Pretty much what it says on the tin. It's a fairly standard API wrapper for [SeaDex].
There's not much interesting to talk about here. By this point it's my third API
wrapper, so I had a decent idea of how to approach it. Development ended up being fairly
uneventful, in a good way.

[SeaDex]: https://releases.moe/about/

## stringenum

*A small, dependency-free library offering additional enum.StrEnum subclasses and a
backport for older Python versions*

[[GitHub]](https://GitHub.com/Ravencentric/stringenum)
[[PyPI]](https://pypi.org/project/stringenum/)

This was mostly a toy project because I wanted to play with [metaclasses]. It backports
features from newer Python versions down to 3.9 and provides a bunch of other string
enums with different properties and guarantees.

The somewhat interesting part here is that the invariants are enforced at construction
time, so you can always rely on them, and it's fully typed. Typing metaclasses was
pretty confusing to wrap my head around, but in the end I got it and this library plays
well with typecheckers.

I wouldn't recommend depending on this library. You should just copy paste the relevant
parts if you really need them.

[metaclasses]: https://docs.python.org/3/reference/datamodel.html#metaclasses

## nzb-rs

*A spec compliant parser for NZB files*

[[GitHub]](https://GitHub.com/Ravencentric/NZB-rs)
[[crates.io]](https://crates.io/crates/NZB-rs)
[[Docs]](https://docs.rs/NZB-rs/latest/NZB_rs/)

Rust had been on my radar for a while, so one day I finally sat down and read the
[Rust book]. Great tooling (cargo), null safety, a powerful type system, pattern
matching - there was a lot going for it. Naturally, I needed a project, and parsers
seemed like exactly the kind of thing Rust excels at. My [NZB parser] was a perfect
candidate: it deals with XML, a format with a long history of security pitfalls that
safe Rust largely avoids by design.

Moving from Python to Rust felt pretty natural, likely thanks to Rust's zero-cost
abstractions that are just as expressive as many Python constructs. Static typing wasn’t
new to me either since I was already using type hints in Python.

Traits feel like a more powerful mix of dunder methods, [`abc.ABC`], and
[`typing.Protocol`]. I miss Rust's enums every time I write Python, and [`Option<T>`]
would make any [PEP-505] fans jealous (myself included).

The borrow checker rarely got in my way. The rules are straightforward: a value has a
single owner, and only one mutable reference at a time. I also barely had to think about
lifetimes since the compiler inferred most of them. Things only got rough when I
experimented with async and started running into more cryptic borrow checker errors, but
that is a story for another time.

The project evolved alongside my understanding of Rust. The [first version] of the
parser was a fairly direct port of the [Python implementation] with an almost identical
API. Even so, the Rust version ended up being several times faster. Some of that came
from Rust itself, but the process also highlighted inefficiencies in the Python
implementation. Taking those lessons back, I was able to remove quite a bit of slower
code and narrow the performance gap. Later versions of the Rust library evolved into a
more idiomatic API. Ironically, the Python implementation was eventually [updated] to be
closer to the Rust API wherever it made sense.

[Rust book]: https://doc.rust-lang.org/book/
[NZB parser]: https://github.com/Ravencentric/nzb
[`abc.ABC`]: https://docs.python.org/3/library/abc.html
[`typing.Protocol`]: https://docs.python.org/3/library/typing.html#typing.Protocol
[`Option<T>`]: https://doc.rust-lang.org/std/option/
[PEP-505]: https://peps.python.org/pep-0505/
[first version]: https://docs.rs/nzb-rs/0.1.0/nzb_rs/
[Python implementation]: https://github.com/Ravencentric/nzb/blob/v0.3.0/src/nzb/_core.py
[updated]: https://github.com/Ravencentric/nzb/releases/tag/v0.4.0

## rnzb

*Python bindings to the NZB-rs library - a spec compliant parser for NZB files, written
in Rust*

[[GitHub]](https://GitHub.com/Ravencentric/rnzb)
[[PyPI]](https://pypi.org/project/rnzb/)

A third NZB parser? Yes. At this point I might have a problem.

After writing one in Python and another in Rust, I had a thought: "Wouldn't it be cool
to use the Rust implementation directly from Python and get the best of both worlds?"
Spoiler alert: yes.

My goal was simple: a drop-in replacement so existing Python code could immediately
benefit from the Rust parser.

After a bit of research I found [`PyO3`], which powers Pydantic and many other
Rust-powered Python extensions. This was my first time writing an extension module, but
thankfully `PyO3` takes care of most of the nitty gritty details and lets me write Rust
like nothing's changed. Honestly, the maintainers have done an amazing job making this
so easy I could hardly believe it. There were a few small hiccups along the way, but the
docs and maintainers were incredibly helpful whenever I had questions.

Once everything worked and the tests passed, the next step was packaging. Wheels are
prebuilt Python packages so users do not have to compile anything during installation.
To build wheels for multiple platforms I used [`pypa/cibuildwheel`], which seems to be
what most projects rely on.

One interesting detail about extension wheels is Python version compatibility. For
example: `rnzb-0.6.0-cp314-cp314-win_arm64.whl`. The `cp314-cp314` tag means the wheel only
works on CPython 3.14. On newer versions the installer falls back to the source
distribution and tries to build it locally, which usually fails unless a Rust toolchain
is installed.

To avoid releasing new wheels for every Python version, I built the extension against
the [Limited C API] (abi3), which `PyO3` supports. The result looks like this:
`rnzb-0.6.0-cp39-abi3-win_amd64.whl`. The `cp39-abi3` tag means the same wheel works on
every CPython version starting from Python 3.9.

So yes, the end result was a third NZB parser. But this one is a little different: it
brings the speed and safety of the Rust implementation to Python while keeping a
familiar drop-in API. You get the Rust parser without changing your code.

[`PyO3`]: https://pyo3.rs/
[`pypa/cibuildwheel`]: https://github.com/pypa/cibuildwheel
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
