
# 1. archivefile
<br/>
<p align="center">
  <a href="https://github.com/Ravencentric/archivefile">
    <img src="https://raw.githubusercontent.com/Ravencentric/archivefile/main/docs/assets/logo.png" alt="Logo" width="400">
  </a>
  <p align="center">
    Unified interface for tar, zip, sevenzip, and rar files
  </p>
</p>

<div align="center">

[![PyPI - Version](https://img.shields.io/pypi/v/archivefile?link=https%3A%2F%2Fpypi.org%2Fproject%2Farchivefile%2F)](https://pypi.org/project/archivefile/)
![PyPI - Python Version](https://img.shields.io/pypi/pyversions/archivefile)
![License](https://img.shields.io/github/license/Ravencentric/archivefile)
![Checked with mypy](https://www.mypy-lang.org/static/mypy_badge.svg)
![Ruff](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/astral-sh/ruff/main/assets/badge/v2.json)

![GitHub Workflow Status (with event)](https://img.shields.io/github/actions/workflow/status/Ravencentric/archivefile/release.yml)
![GitHub Actions Workflow Status](https://img.shields.io/github/actions/workflow/status/ravencentric/archivefile/test.yml?label=tests)
[![codecov](https://codecov.io/gh/Ravencentric/archivefile/graph/badge.svg?token=B45ODO7TEY)](https://codecov.io/gh/Ravencentric/archivefile)

</div>

## Table Of Contents

* [About](#about)
* [Installation](#installation)
* [Docs](#docs)
* [License](#license)

## About

`archivefile` is a wrapper around [`tarfile`](https://docs.python.org/3/library/tarfile.html), [`zipfile`](https://docs.python.org/3/library/zipfile.html), [`py7zr`](https://github.com/miurahr/py7zr), and [`rarfile`](https://github.com/markokr/rarfile).

The above libraries are excellent when you are dealing with a single archive format but things quickly get annoying when you have a bunch of mixed archives such as `.zip`, `.7z`, `.cbr`, `.tar.gz`, etc because each library has a slightly different syntax and quirks which you need to deal with.

`archivefile` wraps the common methods from the above libraries to provide a unified interface that takes care of said differences under the hood. However, it's not as powerful as the libraries it wraps due to lack of support for features that are unique to a specific archive format and library.

## Installation

`archivefile` is available on [PyPI](https://pypi.org/project/archivefile/), so you can simply use [pip](https://github.com/pypa/pip) to install it.

1. Without optional dependencies:

    ```sh
    pip install archivefile
    ```

2. With optional dependencies:

    - Required for [`ArchiveFile.print_tree()`](https://archivefile.ravencentric.cc/api-reference/archivefile/#archivefile.ArchiveFile.print_tree)

        ```sh
        pip install archivefile[bigtree]
        ```

    - Required for [`ArchiveFile.print_table()`](https://archivefile.ravencentric.cc/api-reference/archivefile/#archivefile.ArchiveFile.print_table)

        ```sh
        pip install archivefile[rich]
        ```

3. With all dependencies:

    ```sh
    pip install archivefile[all]
    ```

## Usage

`archivefile` offers a single class called `ArchiveFile` to deal with various archive formats. Some examples are given below:

```py
from archivefile import ArchiveFile

with ArchiveFile("../source.zip") as archive:
    archive.extract("pyproject.toml", destination="./dest/") # Extract a single member by it's name
    archive.extractall(destination="./dest/") # Extract all members
    archive.get_member("pyproject.toml")  # Get the ArchiveMember object for the member by it's name
    archive.get_members()  # Retrieve all members from the archive as a generator of ArchiveMember objects
    archive.get_names()  # Retrieve names of all members in the archive as a tuple of strings
    archive.read_bytes("pyproject.toml") # Read the contents of the member as bytes
    archive.read_text("pyproject.toml")  # Read the contents of the member as text
    archive.print_tree()  # Print the contents of the archive as a tree.
    archive.print_table()  # Print the contents of the archive as a table.

with ArchiveFile("../source.zip", "w") as archive:
    archive.write("foo.txt", arcname="bar.txt")  # Write foo.txt to the archive as bar.txt
    archive.writeall("./src/") # Recursively write the ./src/ directory to the archive
    archive.write_text("spam and eggs", arcname="recipe.txt") # Write a string to the archive as recipe.txt
    archive.write_bytes(b"0101001010100101", arcname="terminator.py")  # Write bytes to the archive as terminator.py
```

## Docs

Checkout the complete documentation [here](https://archivefile.ravencentric.cc/).

## License

Distributed under the [Unlicense](https://choosealicense.com/licenses/unlicense/) License. See [UNLICENSE](https://github.com/Ravencentric/archivefile/blob/main/UNLICENSE) for more information.

---

# 2. atomicwriter
# atomicwriter

[![Tests](https://img.shields.io/github/actions/workflow/status/Ravencentric/atomicwriter/tests.yml?label=tests)](https://github.com/Ravencentric/atomicwriter/actions/workflows/tests.yml)
[![Build](https://img.shields.io/github/actions/workflow/status/Ravencentric/atomicwriter/release.yml?label=build)](https://github.com/Ravencentric/atomicwriter/actions/workflows/release.yml)
![PyPI - Types](https://img.shields.io/pypi/types/atomicwriter)
![License](https://img.shields.io/pypi/l/atomicwriter?color=success)

[![PyPI - Latest Version](https://img.shields.io/pypi/v/atomicwriter?color=blue)](https://pypi.org/project/atomicwriter)
![PyPI - Python Version](https://img.shields.io/pypi/pyversions/atomicwriter)
![PyPI - Implementation](https://img.shields.io/pypi/implementation/atomicwriter)

`atomicwriter` provides the `AtomicWriter` class, which performs cross-platform atomic file writes to ensure "all-or-nothing" operations—if a write fails, the target file is left unchanged.

## Table Of Contents

- [Usage](#usage)
- [Installation](#installation)
- [Building from source](#building-from-source)
- [Acknowledgements](#acknowledgements)
- [License](#license)
- [Contributing](#contributing)

## Usage

```python
from pathlib import Path
from atomicwriter import AtomicWriter

destination = Path("alpha_trion.txt")  # or str

with AtomicWriter(destination) as writer:
    writer.write_text("What defines a Transformer is not the cog in his chest, ")
    writer.write_text("but the Spark that resides in their core.\n")
    assert destination.is_file() is False

assert destination.is_file()
```

Checkout the complete documentation [here](https://ravencentric.cc/atomicwriter/api-reference/).

## Installation

`atomicwriter` is available on [PyPI](https://pypi.org/project/atomicwriter/), so you can simply use pip to install it.

```console
pip install atomicwriter
```

## Building from source

Building from source requires the [Rust toolchain](https://rustup.rs/) and [Python 3.9+](https://www.python.org/downloads/).

- With [`uv`](https://docs.astral.sh/uv/):

  ```console
  git clone https://github.com/Ravencentric/atomicwriter
  cd atomicwriter
  uv build
  ```

- With [`pypa/build`](https://github.com/pypa/build):

  ```console
  git clone https://github.com/Ravencentric/atomicwriter
  cd atomicwriter
  python -m build
  ```

## Acknowledgements

This project is essentially a thin wrapper around the excellent [`tempfile`](https://crates.io/crates/tempfile) crate, which handles all the heavy lifting for atomic file operations.

It is also heavily inspired by the now-archived [`atomicwrites`](https://pypi.org/project/atomicwrites/) project and uses a similar API.

## License

Licensed under either of:

- Apache License, Version 2.0 ([LICENSE-APACHE](https://github.com/Ravencentric/atomicwriter/blob/main/LICENSE-APACHE) or <https://www.apache.org/licenses/LICENSE-2.0>)
- MIT license ([LICENSE-MIT](https://github.com/Ravencentric/atomicwriter/blob/main/LICENSE-MIT) or <https://opensource.org/licenses/MIT>)

at your option.

## Contributing

Contributions are welcome! Unless you explicitly state otherwise, any contribution intentionally submitted for inclusion in the work by you, as defined in the Apache-2.0 license, shall be dual licensed as above, without any additional terms or conditions.

---

# 3. awesome-arr
# Awesome *Arr [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> Lidarr, Prowlarr, Radarr, Sonarr, and Whisparr are collectively referred to as "*arr" or "*arrs". They are designed to automatically grab, sort, organize, and monitor your Music, Movie, E-Book, or TV Show collections for Lidarr, Radarr, Sonarr, and Whisparr; and to manage your indexers and keep them in sync with the aforementioned apps for Prowlarr. This list aims to list all *arrs and things related to them

## Contents

- [Primary \*arrs](#primary-arrs)
- [Indexer Managers](#indexer-managers)
- [Resources](#resources)
- [\*arrs with Additional Functionality](#arrs-with-additional-functionality)
- [\*arr Alternatives](#arr-alternatives)
- [Complimenting Apps](#complimenting-apps)
- [Bots](#bots)
- [Dashboards](#dashboards)
- [Mobile Apps](#mobile-apps)

## Primary *arrs

> These are collection managers for Usenet and BitTorrent users. They can monitor multiple RSS feeds for new content and will grab, sort, and rename them. They can also be configured to automatically upgrade the quality of files already downloaded when a better quality format becomes available.

- [Lidarr](https://lidarr.audio/) - Lidarr is a music collection manager.
- [Radarr](https://radarr.video/) - Radarr is a movie collection manager.
- [Sonarr](https://sonarr.tv/) - Smart PVR for newsgroup and bittorrent users.
- [Whisparr](https://whisparr.com/) - Whisparr is an adult movie collection manager.

## Indexer Managers

- [Prowlarr](https://github.com/prowlarr/prowlarr) - Prowlarr is an indexer manager/proxy built on the popular arr .net/reactjs base stack to integrate with your various PVR apps. Prowlarr supports management of both Torrent Trackers and Usenet Indexers. It integrates seamlessly with Lidarr, Mylar3, Radarr, and Sonarr offering complete management of your indexers with no per app Indexer setup required.
- [Jackett](https://github.com/Jackett/Jackett) - API Support for your favorite torrent trackers. An alternative to Prowlarr.

## Resources

- [Servarr](https://wiki.servarr.com/) -  The consolidated wiki for Lidarr, Prowlarr, Radarr, and Sonarr.
- [TRaSH-Guides](https://trash-guides.info/) - Guides mainly for Sonarr/Radarr/Bazarr and everything related to it.

## *arrs with Additional Functionality

- [Docker Lidarr Extended](https://github.com/RandomNinjaAtk/docker-lidarr-extended) - Lidarr application packaged with multiple scripts to provide additional functionality.
- [Docker Radarr Extended](https://github.com/RandomNinjaAtk/docker-radarr-extended) - Radarr (develop) with bash scripts to automate and extend functionality.
- [Docker Sonarr Extended](https://github.com/RandomNinjaAtk/docker-sonarr-extended) - Sonarr (develop) with bash scripts to automate and extend functionality.
- [Lidarr on Steroids](https://github.com/youegraillot/lidarr-on-steroids) - This repository bundles a modded version of Lidarr and Deemix into a Docker image.

## *arr Alternatives

> These work similarly to an *arr and serve as an alternative to them.

- [Flexget](https://github.com/Flexget/Flexget) - FlexGet is a multipurpose automation tool for all of your media. Support for torrents, nzbs, podcasts, comics, TV, movies, RSS, HTML, CSV, and more.
- [Kapowarr](https://github.com/Casvt/Kapowarr) - Kapowarr is a software to build and manage a comic book library.
- [Medusa](https://pymedusa.com/) - Medusa is an automatic Video Library Manager for TV Shows. It watches for new episodes of your favorite shows, and when they are posted it does its magic: automatic torrent/nzb searching, downloading, and processing at the qualities you want.
- [Mylar3](https://github.com/mylar3/mylar3) - The python3 version of the automated Comic Book downloader (cbr/cbz) for use with various download clients.
- [SickGear](https://github.com/SickGear/SickGear) - SickGear has proven the most reliable stable TV fork of the great Sick-Beard to fully automate TV enjoyment with innovation.
- [SoulSync](https://github.com/Nezreka/SoulSync) - Intelligent Music Discovery & Automation Platform. Automates downloads, curates playlists, monitors artists, and organizes your collection

## Complimenting Apps

- [Arr-scripts](https://github.com/RandomNinjaAtk/arr-scripts) - Extended Container Scripts. Designed to be easily implemented/added to Linuxserver.io containers.
- [Autobrr](https://autobrr.com/) - The modern autodl-irssi replacement.
- [Autopulse](https://github.com/dan-online/autopulse) - An automated lightweight service that updates media servers like Plex and Jellyfin based on notifications from media organizers like Sonarr and Radarr.
- [Autoscan](https://github.com/Cloudbox/autoscan) - Autoscan replaces the default Plex and Emby behaviour for picking up changes on the file system.
- [Bazarr](https://github.com/morpheus65535/bazarr) - Bazarr is a companion application to Sonarr and Radarr. It manages and downloads subtitles based on your requirements. You define your preferences by TV show or movie and Bazarr takes care of everything for you.
- [Buildarr](https://github.com/buildarr/buildarr) - A solution to automating deployment and configuration of your *arr stack.
- [Byparr](https://github.com/ThePhaseless/Byparr/) - An alternative to FlareSolverr as a drop-in replacement, built with seleniumbase and FastAPI.
- [Calendarr](https://github.com/jordanlambrecht/calendarr) - A notification system that sends scheduled Sonarr/Radarr calendar updates to Discord and Slack.
- [Checkrr](https://github.com/aetaric/checkrr) - Checkrr Scans your library files for corrupt media and replace the files via sonarr and radarr.
- [Cleanarr (hrenard)](https://github.com/hrenard/cleanarr/) - A small utility tasked to automatically clean radarr and sonarr files over time.
- [Cleanarr (se1exin)](https://github.com/se1exin/Cleanarr) - A simple UI to help find and delete duplicate and sample files from your Plex server.
- [Cleanuparr](https://github.com/Cleanuparr/Cleanuparr) - An advanced cleaner for dead or malicious torrents.
- [Cloud Seeder](https://ipv6.rs/cloudseeder) - 1 click installer and updater for Prowlarr, Lidarr, Radarr, Sonarr and Whisparr. Also links and connects qBittorrent.
- [Collectarr](https://github.com/RiffSphere/Collectarr) - A Python script for checking your Radarr database and setting up collection lists. Also supports "smart" actor lists based on TMDB.
- [Crossarr](https://github.com/TMD20/crossarr) - Cross Seed via Arr Programs.
- [Dasharr](https://github.com/Arcadia-Solutions/Dasharr) - Dashboard of torrent indexers usage, profile stats evolution over time.
- [Decluttarr](https://github.com/ManiMatter/decluttarr) - Watches radarr, sonarr, lidarr and whisparr download queues and removes downloads if they become stalled or no longer needed.
- [Deleterr](https://github.com/rfsbraz/deleterr) - Automates deleting inactive and stale media from Plex/Sonarr/Radarr.
- [Deployarr](https://github.com/anandslab/deployarr) - Deployarr automates Homelab setup using Docker and Docker Compose.
- [Elsewherr](https://github.com/Adman1020/Elsewherr) - See disclaimer on page. See if your movies from Radarr are available on a streaming service, and add a tag against the movie if it is.
- [Episeerr](https://github.com/Vansmak/episeerr) - Automates sending and deleting episodes or seasons to sonarr one at a time as played. 
- [Excludarr](https://github.com/haijeploeg/excludarr) - Excludarr is a CLI that interacts with Radarr and Sonarr instances. It completely manages you library in Sonarr and Radarr to only consist out of movies and series that are not present on any of the configured streaming providers.
- [Exportarr](https://github.com/onedr0p/exportarr) - This will export metrics gathered from Sonarr, Radarr, Lidarr, or Prowlarr.
- [Ezarr](https://github.com/Luctia/ezarr) - Ezarr aims to make it as easy as possible to setup an entire Servarr/Jackett/BitTorrent/PleX/Jellyfin mediacenter stack using Docker.
- [Nixarr](https://github.com/rasmus-kirk/nixarr) - Nixarr is a Nixos module that helps setup and manage a media server stack natively in Nixos. Supports a lot of *Arrs, Jellyfin, Plex, Audiobookshelf, has both Usenet and Torrent modules and has built-in VPN-support.
- [FlareSolverr](https://github.com/FlareSolverr/FlareSolverr) - Proxy server to bypass Cloudflare protection.
- [Flemmarr](https://github.com/Flemmarr/Flemmarr) - Flemmarr makes it easy to automate configuration for your -arr apps.
- [Gclone](https://github.com/l3v11/gclone) - A rclone mod with auto SA rotation.
- [iPlayarr](https://github.com/Nikorag/iplayarr) – Download-automation for BBC iPlayer — integrates with Sonarr/Radarr as a Newznab API endpoint.
- [Janitorr](https://github.com/Schaka/janitorr) - Cleans your Radarr, Sonarr, Jellyseerr and Jellyfin before you run out of space.
- [Jellyseerr](https://github.com/Fallenbagel/jellyseerr) - Open-source media request and discovery manager for Jellyfin, Plex and Emby.
- [Just A Bunch Of Starr Scripts](https://github.com/angrycuban13/Just-A-Bunch-Of-Starr-Scripts) - PowerShell scripts for Starr apps.
- [Kometa](https://github.com/Kometa-Team/Kometa) - Kometa (formerly Plex Meta Manager) is an open source Python 3 project that has been designed to ease the creation and maintenance of metadata, collections, and playlists within a Plex Media Server.
- [Labelarr](https://github.com/nullable-eth/labelarr) - Application that bridges your Plex media libraries with The Movie Database, adding relevant keywords as searchable labels or genres for custom filters in Plex.
- [Letterboxd List Radarr](https://github.com/screeny05/letterboxd-list-radarr) - Connect Radarr to letterboxd.com lists.
- [Lingarr](https://github.com/lingarr-translate/lingarr) - Lingarr integrates with Radarr and Sonarr and automates subtitle translation using various locally hosted or SaaS translation services.
- [Linkarr](https://github.com/itsmejoeeey/linkarr) - Automatically organize your media library without moving or duplicating the source files.
- [Listrr](https://github.com/TheUltimateC0der/Listrr) - Listrr creates lists for shows and movies based on your filters. The created lists get updated every 24 hours based on your filters, so Listrr will add all new items that match your filters, and will also remove all items that do not match your filter configuration anymore. Supports Sonarr, Radarr, Traktarr and Python-PlexLibrary.
- [Maintainerr](https://github.com/jorenn92/Maintainerr) - Looks and smells like Overseerr, does the opposite. Maintenance tool for the Plex ecosystem.
- [Managarr](https://github.com/Dark-Alex-17/managarr) - A TUI and CLI to help you manage all your Servarrs.
- [Mediarr](https://github.com/l3uddz/mediarr) - CLI tool to add new media to pvr's from the arr suite.
- [MediathekArr](https://github.com/PCJones/MediathekArr/) -  Integrate ARD&ZDF Mediathek in Prowlarr, Sonarr, and Radarr (German free public TV stations).
- [Midarr](https://github.com/midarrlabs/midarr-server) - Midarr, the minimal lightweight media server.
- [Monitorr](https://github.com/Monitorr/Monitorr) - Monitorr is a self-hosted PHP web app that monitors the status of local and remote network services, websites, and applications.
- [Notifiarr](https://notifiarr.com/) - Discord notification system.
- [Ombi](https://github.com/Ombi-app/Ombi) - Ombi is a self-hosted web application that automatically gives your shared Plex or Emby users the ability to request content by themselves! Ombi can be linked to multiple TV Show and Movie DVR tools to create a seamless end-to-end experience for your users.
- [Overseerr](https://overseerr.dev/) - Request management and media discovery tool for the Plex ecosystem.
- [Plexist](https://github.com/Gyarbij/Plexist) - An application for recreating Spotify and Deezer playlist in Plex.
- [Plundrio](https://github.com/elsbrock/plundrio) - A put.io download client for *arr implementing the transmission RPC interface.
- [Posteria](https://github.com/jeremehancock/Posteria) - A sleek, modern solution for managing your movie, TV show, and collection posters.
- [Posterizarr](https://github.com/fscorrupt/Posterizarr) - Automated poster maker for Plex/Jellyfin/Emby.
- [Posterr](https://github.com/petersem/posterr) - A digital poster app for Plex, Sonarr and Radarr.
- [Prefetcharr](https://github.com/p-hueber/prefetcharr) - Let Sonarr fetch the next season of a show you are watching on Jellyfin/Emby/Plex. 
- [Profilarr](https://github.com/santiagosayshey/Profilarr) - Import, Export & Sync Profiles & Custom Formats via Radarr / Sonarr API.
- [Proxarr](https://github.com/Fazzani/Proxarr) - Prevents Sonarr/Radarr from downloading media already available for your region on streaming services (e.g., Netflix, Amazon Prime Video)
- [Prunerr](https://github.com/rpatterson/prunerr) - Perma-seed Servarr media libraries.
- [Pulsarr](https://github.com/jamcalli/Pulsarr) - An integration tool that bridges Plex watchlists with Sonarr and Radarr, enabling real-time media monitoring and automated content acquisition all from within the Plex App itself.
- [Quasarr](https://github.com/rix1337/Quasarr) - Quasarr connects JDownloader with Radarr, Sonarr, Lidarr and LazyLibrarian. It also decrypts links protected by CAPTCHAs.
- [Radarr-striptracks](https://github.com/linuxserver/docker-mods/tree/radarr-striptracks) - A Docker Mod for the LinuxServer.io Radarr/Sonarr v3 Docker container that adds a script to automatically strip out unwanted audio and subtitle streams, keeping only the desired languages.
- [Rclone](https://rclone.org/) - Rclone is a command-line program to manage files on cloud storage.
- [Recommendarr](https://github.com/fingerthief/recommendarr) - An AI driven recommendation system based on Radarr and Sonarr library information.
- [Recyclarr](https://github.com/recyclarr/recyclarr) - Automatically sync TRaSH guides to your Sonarr and Radarr instances.
- [Reiverr](https://github.com/aleksilassila/reiverr) - Reiverr is a clean combined interface for Jellyfin, TMDB, Radarr and Sonarr, as well as a replacement to Overseerr.
- [Scraparr](https://github.com/thecfu/scraparr) - This will generate Prometheus Metrics gathered from various *arr and *arr-stack applications.
- [Sonarr Episode Name Checker](https://github.com/tronyx/sonarr-episode-name-checker) - Bash and Powershell scripts to check for episodes named "Episode ##" or "TBA".
- [Soularr](https://github.com/mrusse/soularr) - A Python script that connects Lidarr with Soulseek. 
- [StarrScripts](https://github.com/bakerboy448/StarrScripts) - Misc scripts for starr related apps.
- [SuggestArr](https://github.com/giuseppe99barchetta/SuggestArr) - Automatic media content recommendations and download requests based on user activity on the media server.
- [Taggarr](https://github.com/BassHous3/taggarr) - Dub analysis and tagging for media management and filtering your favourite shows in dub. 
- [Tdarr](https://github.com/HaveAGitGat/Tdarr) - Distributed transcode automation using FFmpeg/HandBrake + Audio/Video library analytics + video health checking.
- [Toolbarr](https://github.com/Notifiarr/toolbarr) - Provides a suite of utilities to fix problems with Starr applications. Toolbarr allows you to perform various actions against your Starr apps and their SQLite3 databases.
- [Tracearr](https://github.com/connorgallopo/Tracearr) - Real-time monitoring platform for Plex, Jellyfin, and Emby. Track streams, playback analytics, and account sharing detection in one place.
- [Trailarr](https://github.com/nandyalu/trailarr) - A Docker application to download and manage trailers for your Radarr, and Sonarr libraries.
- [Traktarr](https://github.com/l3uddz/traktarr) - Script to add new series & movies to Sonarr/Radarr based on Trakt lists.
- [Transcoderr](https://github.com/drkno/transcoderr) - A transcoding pipeline designed to normalize file types into a common filetype. Dynamically configurable using plugins allowing highly customizable pipelines to be built.
- [UmlautAdaptarr](https://github.com/PCJones/UmlautAdaptarr) - A tool to work around Sonarr, Radarr and Lidarr problems with foreign languages (primarily German at the moment).
- [Unpackerr](https://github.com/davidnewhall/unpackerr) - Extracts downloads for Radarr, Sonarr, and Lidarr - Deletes extracted files after import.
- [Watchlistarr](https://github.com/nylonee/watchlistarr) - Automatically sync Plex Watchlists with Sonarr and Radarr.
- [Wizarr](https://github.com/Wizarrrr/wizarr) - Wizarr is an automatic user invitation system for Plex, Jellyfin, Emby, AudiobookShelf, Komga, Kavita and Romm.
- [Wrapperr](https://github.com/aunefyren/wrapperr) - Website based application and API that collects Plex statistics using Tautulli and displays it in a nice format. Similar to the Spotify Wrapped concept.

## Bots

- [Addarr](https://github.com/Waterboy1602/Addarr) - Telegram Bot for adding series/movies to Sonarr/Radarr or for changing the download speed of Transmission/Sabnzbd.
- [Botdarr](https://github.com/shayaantx/botdarr) - Slack/Discord/Telegram/Matrix bot for accessing radarr, sonarr, and lidarr.
- [Doplarr](https://github.com/kiranshila/Doplarr) - An *arr request bot for Discord.
- [Invitarr](https://github.com/Sleepingpirates/Invitarr) - Invitarr is a chatbot that invites discord users to plex.
- [jackett2telegram](https://github.com/danimart1991/jackett2telegram) - A self-hosted Telegram Python Bot that dumps posts from Jackett RSS feeds to a Telegram chat.
- [Membarr](https://github.com/Yoruio/Membarr) - Discord Bot to invite a user to a Plex or Jellyfin server.
- [Requestrr](https://github.com/darkalfx/requestrr) - Requestrr is a Discord bot used to simplify using services like Sonarr/Radarr/Ombi via the use of chat.
- [Searcharr](https://github.com/toddrob99/searcharr) - Sonarr & Radarr Telegram Bot.

## Dashboards

> These are dashboards for your *arrs and various other services on your server.

- [Dashy](https://github.com/Lissy93/dashy) - A self-hostable personal dashboard built for you. Includes status-checking, widgets, themes, icon packs, a UI editor and tons more.
  - Note, Dashy has not received any new release in over a year. Consider alternatives if you need active development and support.
- [Flame](https://github.com/pawelmalak/flame) - Flame is self-hosted startpage for your server. Easily manage your apps and bookmarks with built-in editors.
- [Heimdall](https://github.com/linuxserver/Heimdall) - An Application dashboard and launcher.
- [Homarr](https://github.com/homarr-labs/homarr) - A simple, yet powerful dashboard for your server. A sleek, modern dashboard that puts all of your apps and services at your fingertips.
- [Homepage](https://github.com/gethomepage/homepage) - A highly customizable homepage (or startpage / application dashboard) with Docker and service API integration.
- [Homer](https://github.com/bastienwirtz/homer) - A very simple static homepage for your server with offline health check.
- [Organizr](https://github.com/causefx/Organizr) - HTPC/Homelab Services Organizer - Written in PHP.

## Mobile Apps

- [Ruddarr](https://github.com/ruddarr/app) - Ruddarr is a beautifully designed, open source, iOS/iPadOS companion app for Radarr and Sonarr instances written in SwiftUI.
- [nzb360](https://nzb360.com/) - Usenet/Torrent manager for Android. Supports SABnzbd, NZBget, Deluge, Transmission, uTorrent, qBittorrent, rTorrent/ruTorrent, Sonarr, Sick Beard, Radarr, Lidarr, Bazarr, Couchpotato, Headphones, NEWZnab, Jackett, NZBHydra2 and Prowlarr.

---

# 4. juicenet-cli
<br/>
<p align="center">
  <a href="https://github.com/Ravencentric/juicenet-cli">
    <img src="https://raw.githubusercontent.com/Ravencentric/juicenet-cli/main/docs/assets/logo.png" alt="Logo" width="100" height="100">
  </a>

  <h3 align="center">juicenet</h3>

  <p align="center">
    CLI tool designed to simplify the process of uploading files to Usenet
    <br/>
    <br/>
  </p>
</p>

<div align="center">

[![PyPI - Version](https://img.shields.io/pypi/v/juicenet-cli?link=https%3A%2F%2Fpypi.org%2Fproject%2Fjuicenet-cli%2F)](https://pypi.org/project/juicenet-cli/)
![PyPI - Python Version](https://img.shields.io/pypi/pyversions/juicenet-cli)
![GitHub Workflow Status (with event)](https://img.shields.io/github/actions/workflow/status/Ravencentric/juicenet-cli/release.yml)
[![GitHub Workflow Status (with event)](https://img.shields.io/github/actions/workflow/status/ravencentric/juicenet-cli/docker.yml?label=docker)](https://hub.docker.com/r/ravencentric/juicenet-cli)
![License](https://img.shields.io/github/license/Ravencentric/juicenet-cli)
![Checked with mypy](https://www.mypy-lang.org/static/mypy_badge.svg)
![Ruff](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/astral-sh/ruff/main/assets/badge/v2.json)

</div>

## Table Of Contents

* [About the Project](#about-the-project)
* [Installation](#installation)
  * [Local](#local)
  * [Docker](#docker)
* [Docs](#docs)
* [License](#license)

## About The Project

Uploading stuff to Usenet is tedious so I tried to make it easier.

* Uses [ParPar](https://github.com/animetosho/ParPar) and [Nyuu](https://github.com/animetosho/Nyuu) under the hood
* Recursively searches for files with pre-defined extensions in `juicenet.yaml` or as passed in `--exts`
* Alternatively, searches for glob patterns passed in `--glob`
* Preserves folder structure without RAR. [RAR sucks and here's why](https://github.com/animetosho/Nyuu/wiki/Stop-RAR-Uploads)
* Does everything automatically and gives you the resulting `nzbs` in a neatly sorted manner
* Offers the option to pick and choose what it does if you don't want it doing everything automatically
* Automatically checks for and reposts failed articles from last run. Also has the option to not do this.
* Can continue from where it stopped if it gets interrupted for any reason

## Installation

### Local

#### Prerequisites

* [Python >= 3.9](https://www.python.org/downloads/)
* [ParPar >= 0.4.2](https://github.com/animetosho/ParPar)
* [Nyuu >= 0.4.2](https://github.com/animetosho/Nyuu)

#### Installation

1. With [pipx](https://pypa.github.io/pipx/installation/) (recommended):

    ```sh
    pipx install juicenet-cli
    ```

2. With pip:

    ```sh
    pip install juicenet-cli
    ```

For more details, checkout the local installation guide [here](https://juicenet.ravencentric.cc/installation/local/)

### Docker

```yaml
---
version: "2.1"
services:
  juicenet:
    image: ravencentric/juicenet-cli:latest
    container_name: juicenet
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=Etc/UTC
    volumes:
      - host/path/to/config/nyuu.docker.private.json:/config/nyuu.docker.private.json
      - host/path/to/config/nyuu.docker.public.json:/config/nyuu.docker.public.json
      - host/path/to/data/nzbs:/data/nzbs
      - host/path/to/data/appdata:/data/appdata
      - host/path/to/data/raw:/data/raw
      - host/path/to/media:/media
```

```shell
docker compose -f "path/to/docker-compose.yml" run juicenet --help
```

For more details, checkout the docker installation guide [here](https://juicenet.ravencentric.cc/installation/docker/)

## Docs

Checkout the complete documentation [here](https://juicenet.ravencentric.cc/)

## License

Distributed under the [Unlicense](https://choosealicense.com/licenses/unlicense/) License. See [UNLICENSE](https://github.com/Ravencentric/juicenet-cli/blob/main/UNLICENSE) for more information.

---

# 5. misaki
# misaki
![Crates.io Version](https://img.shields.io/crates/v/misaki-core)
![docs.rs](https://img.shields.io/docsrs/misaki-core)
![Crates.io License](https://img.shields.io/crates/l/misaki-core)

misaki is a fast, asynchronous link checker with optional [FlareSolverr](https://github.com/FlareSolverr/FlareSolverr) support.

This repository contains two crates:

- [`misaki-core`](https://crates.io/crates/misaki-core): The core library that provides the link-checking functionality.
- [`misaki-cli`](https://crates.io/crates/misaki-cli): A command-line interface for `misaki-core`.

## misaki-core

`misaki-core` is a library for checking the status of URLs asynchronously.

### Usage

Add `misaki-core` to your `Cargo.toml`:

```console
$ cargo add misaki-core
```

Here is an example of how to use `misaki-core`:

```rust
use anyhow::Result;
use futures::StreamExt;
use misaki_core::LinkChecker;

#[tokio::main]
async fn main() -> Result<()> {
    let urls = vec!["https://httpbin.org/status/200"; 10];
    let checker = LinkChecker::builder().build().await?;
    {
        let iter = checker.check_all(urls).await;
        let mut iter = std::pin::pin!(iter);

        while let Some(status) = iter.next().await {
            println!("{:?}", status);
        }
    }
    checker.close().await?;
    Ok(())
}
```

## misaki-cli

`misaki-cli` is a command-line tool for checking links.

### Installation

You can install `misaki-cli` from crates.io using cargo:

```bash
cargo install misaki-cli
```

Alternatively, pre-built binaries for various platforms are available on the [GitHub releases page](https://github.com/Ravencentric/misaki/releases/latest).

### Usage

You can pipe a list of URLs to `misaki` to check them:

```bash
cat urls.txt | misaki -
```

Or, you can pass a single URL directly as an argument:

```bash
misaki https://google.com
```

#### With FlareSolverr

To use FlareSolverr, provide the base URL of your FlareSolverr instance using the `--flaresolverr` flag.

```bash
cat urls.txt | misaki - --flaresolverr http://localhost:8191
```

## License

This project is licensed under either of

- Apache License, Version 2.0, (LICENSE-APACHE or <http://www.apache.org/licenses/LICENSE-2.0>)
- MIT license (LICENSE-MIT or <http://opensource.org/licenses/MIT>)

at your option.

## Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted for inclusion in the work by you, as defined in the Apache-2.0 license, shall be dual licensed as above, without any additional terms or conditions.

---

# 6. mkvinfo
# mkvinfo

[![PyPI - Version](https://img.shields.io/pypi/v/mkvinfo?link=https%3A%2F%2Fpypi.org%2Fproject%2Fmkvinfo%2F)](https://pypi.org/project/mkvinfo/)
![PyPI - Python Version](https://img.shields.io/pypi/pyversions/mkvinfo)
![License](https://img.shields.io/github/license/Ravencentric/mkvinfo)
![PyPI - Types](https://img.shields.io/pypi/types/mkvinfo)

![GitHub Build Workflow Status](https://img.shields.io/github/actions/workflow/status/Ravencentric/mkvinfo/release.yml)
![GitHub Tests Workflow Status](https://img.shields.io/github/actions/workflow/status/ravencentric/mkvinfo/tests.yml?label=tests)
[![codecov](https://codecov.io/gh/Ravencentric/mkvinfo/graph/badge.svg?token=96UDXZHP41)](https://codecov.io/gh/Ravencentric/mkvinfo)

Python library for probing [matroska](https://www.matroska.org/index.html) files with [`mkvmerge`](https://mkvtoolnix.download/doc/mkvmerge.html).

## Installation

`mkvinfo` is available on [PyPI](https://pypi.org/project/mkvinfo/), so you can simply use [pip](https://github.com/pypa/pip) to install it.

```sh
pip install mkvinfo
```

## Usage

```py
from mkvinfo import MKVInfo

mkv = MKVInfo.from_file("./Big Buck Bunny, Sunflower version.mkv")

assert mkv.file_name == "Big Buck Bunny, Sunflower version.mkv"
assert mkv.container.properties.title == "Big Buck Bunny, Sunflower version"
assert mkv.container.properties.writing_application == "mkvmerge v92.0 ('Everglow') 64-bit"

for track in mkv.tracks:
    print(f"{track.id} - {track.codec}")
    #> 0 - AVC/H.264/MPEG-4p10
    #> 1 - MP3
    #> 2 - AC-3
```

Checkout the complete documentation [here](https://ravencentric.cc/mkvinfo/).

## License

Distributed under the [MIT](https://choosealicense.com/licenses/mit/) License. See [LICENSE](https://github.com/Ravencentric/mkvinfo/blob/main/LICENSE) for more information.
---

# 7. myne
# myne

[![Tests](https://img.shields.io/github/actions/workflow/status/Ravencentric/myne/tests.yml?label=tests)](https://github.com/Ravencentric/myne/actions/workflows/tests.yml)
[![Build](https://img.shields.io/github/actions/workflow/status/Ravencentric/myne/release.yml?label=build)](https://github.com/Ravencentric/myne/actions/workflows/release.yml)
![PyPI - Types](https://img.shields.io/pypi/types/myne)
![License](https://img.shields.io/pypi/l/myne?color=success)

[![PyPI - Latest Version](https://img.shields.io/pypi/v/myne?color=blue)](https://pypi.org/project/myne)
![PyPI - Python Version](https://img.shields.io/pypi/pyversions/myne)
![PyPI - Implementation](https://img.shields.io/pypi/implementation/myne)

`myne` is a parser for manga and light novel filenames, providing the `Book` class for extracting and representing metadata such as title, volume, chapter, edition, and more.

## Usage

```python
from myne import Book

book = Book.parse("Ascendance of a Bookworm - v07 (2021) (Digital) (Kinoworm).cbz")

assert book.title == "Ascendance of a Bookworm"
assert book.volume == "7"
assert book.year == 2021
assert book.digital is True
assert book.group == "Kinoworm"
```

Checkout the complete documentation [here](https://ravencentric.cc/myne/).

## Installation

`myne` is available on [PyPI](https://pypi.org/project/myne/), so you can simply use pip to install it.

```console
pip install myne
```

## Building from source

Building from source requires the [Rust toolchain](https://rustup.rs/) and [Python 3.9+](https://www.python.org/downloads/).

- With [`uv`](https://docs.astral.sh/uv/):

  ```console
  git clone https://github.com/Ravencentric/myne
  cd myne
  uv build
  ```

- With [`pypa/build`](https://github.com/pypa/build):

  ```console
  git clone https://github.com/Ravencentric/myne
  cd myne
  python -m build
  ```

## License

Licensed under either of:

- Apache License, Version 2.0 ([LICENSE-APACHE](https://github.com/Ravencentric/myne/blob/main/LICENSE-APACHE) or <https://www.apache.org/licenses/LICENSE-2.0>)
- MIT license ([LICENSE-MIT](https://github.com/Ravencentric/myne/blob/main/LICENSE-MIT) or <https://opensource.org/licenses/MIT>)

at your option.

## Contributing

Contributions are welcome! Unless you explicitly state otherwise, any contribution intentionally submitted for inclusion in the work by you, as defined in the Apache-2.0 license, shall be dual licensed as above, without any additional terms or conditions.

---

# 8. nzb
# nzb

[![PyPI - Version](https://img.shields.io/pypi/v/nzb?link=https%3A%2F%2Fpypi.org%2Fproject%2Fnzb%2F)](https://pypi.org/project/nzb/)
![PyPI - Python Version](https://img.shields.io/pypi/pyversions/nzb)
![License](https://img.shields.io/github/license/Ravencentric/nzb)
![PyPI - Types](https://img.shields.io/pypi/types/nzb)

![GitHub Build Workflow Status](https://img.shields.io/github/actions/workflow/status/Ravencentric/nzb/release.yml)
![GitHub Tests Workflow Status](https://img.shields.io/github/actions/workflow/status/ravencentric/nzb/tests.yml?label=tests)
[![Codecov Status](https://codecov.io/gh/Ravencentric/nzb/graph/badge.svg?token=FFSOFFOM6J)](https://codecov.io/gh/Ravencentric/nzb)

A [spec](https://sabnzbd.org/wiki/extra/nzb-spec) compliant parser and meta editor for NZB files.

## Installation

`nzb` is available on [PyPI](https://pypi.org/project/nzb/), so you can simply use [pip](https://github.com/pypa/pip) to install it.

```sh
pip install nzb
```

## Docs

Checkout the [tutorial](https://ravencentric.cc/nzb/tutorial/) and the [API reference](https://ravencentric.cc/nzb/api-reference/parser/).

## License

Distributed under the [MIT](https://choosealicense.com/licenses/mit/) License. See [LICENSE](https://github.com/Ravencentric/nzb/blob/main/LICENSE) for more information.

---

# 9. nzb-rs
nzb-rs
========

[![Tests](https://img.shields.io/github/actions/workflow/status/Ravencentric/nzb-rs/tests.yml?label=tests)](https://github.com/Ravencentric/nzb-rs/actions/workflows/tests.yml)
[![Latest Version](https://img.shields.io/crates/v/nzb-rs)](https://crates.io/crates/nzb-rs)
[![Documentation](https://docs.rs/nzb-rs/badge.svg)](https://docs.rs/nzb-rs)
![License](https://img.shields.io/crates/l/nzb-rs)

`nzb-rs` is a [spec](https://sabnzbd.org/wiki/extra/nzb-spec) compliant parser for [NZB](https://en.wikipedia.org/wiki/NZB) files.

## Installation

`nzb-rs` is available on [crates.io](https://crates.io/crates/nzb-rs), so you can simply use [cargo](https://github.com/rust-lang/cargo) to install it.

```console
cargo add nzb-rs
```

Optional features:

- `serde`: Enables serialization and deserialization via [serde](https://crates.io/crates/serde).

## Example

```rust
use nzb_rs::{Nzb, ParseNzbError};

fn main() -> Result<(), ParseNzbError> {
    let xml = r#"
        <?xml version="1.0" encoding="UTF-8"?>
        <!DOCTYPE nzb PUBLIC "-//newzBin//DTD NZB 1.1//EN" "http://www.newzbin.com/DTD/nzb/nzb-1.1.dtd">
        <nzb
            xmlns="http://www.newzbin.com/DTD/2003/nzb">
            <file poster="John &lt;nzb@nowhere.example&gt;" date="1706440708" subject="[1/1] - &quot;Big Buck Bunny - S01E01.mkv&quot; yEnc (1/2) 1478616">
                <groups>
                    <group>alt.binaries.boneless</group>
                </groups>
                <segments>
                    <segment bytes="739067" number="1">9cacde4c986547369becbf97003fb2c5-9483514693959@example</segment>
                    <segment bytes="739549" number="2">70a3a038ce324e618e2751e063d6a036-7285710986748@example</segment>
                </segments>
            </file>
        </nzb>
        "#;
    let nzb = Nzb::parse(xml)?;
    println!("{:#?}", nzb);
    assert_eq!(nzb.file().name(), Some("Big Buck Bunny - S01E01.mkv"));
    Ok(())
}
```

## Safety

- This library must not panic. Any panic should be considered a bug and reported.
- This library uses [`roxmltree`](https://crates.io/crates/roxmltree) for parsing the NZB. `roxmltree` is written entirely in safe Rust, so by Rust's guarantees the worst that a malicious NZB can do is to cause a panic.

## Acknowledgements

Some parts of this project's codebase, including parsing-related logic and
associated test cases, are inspired by or derived from
[SABnzbd](https://github.com/sabnzbd/sabnzbd).

Thanks to the SABnzbd project and its contributors for their work. Related
sections of code are annotated with comments pointing back to the original
SABnzbd sources where appropriate.

## License

Licensed under either of

 * Apache License, Version 2.0
   ([LICENSE-APACHE](https://github.com/Ravencentric/nzb-rs/blob/main/LICENSE-APACHE) or <https://www.apache.org/licenses/LICENSE-2.0>)
 * MIT license
   ([LICENSE-MIT](https://github.com/Ravencentric/nzb-rs/blob/main/LICENSE-MIT) or <https://opensource.org/license/MIT>)

at your option.

## Contribution

Unless you explicitly state otherwise, any contribution intentionally submitted
for inclusion in the work by you, as defined in the Apache-2.0 license, shall be
dual licensed as above, without any additional terms or conditions.

---

# 10. privatebin
# privatebin

[![PyPI - Version](https://img.shields.io/pypi/v/privatebin?link=https%3A%2F%2Fpypi.org%2Fproject%2Fprivatebin%2F)](https://pypi.org/project/privatebin/)
![PyPI - Python Version](https://img.shields.io/pypi/pyversions/privatebin)
![License](https://img.shields.io/github/license/Ravencentric/privatebin)
![PyPI - Types](https://img.shields.io/pypi/types/privatebin)

![GitHub Build Workflow Status](https://img.shields.io/github/actions/workflow/status/Ravencentric/privatebin/release.yml)
![GitHub Tests Workflow Status](https://img.shields.io/github/actions/workflow/status/ravencentric/privatebin/tests.yml?label=tests)
[![codecov](https://codecov.io/gh/Ravencentric/privatebin/graph/badge.svg?token=L1ZPQCVNDG)](https://codecov.io/gh/Ravencentric/privatebin)

Python library for interacting with PrivateBin's v2 API (PrivateBin >= 1.3) to create, retrieve, and delete encrypted pastes.

## Installation

`privatebin` is available on [PyPI](https://pypi.org/project/privatebin/), so you can simply use [pip](https://github.com/pypa/pip) to install it.

1. To install the core library:

    ```sh
    pip install privatebin
    ```

2. To install the CLI:

    - With [`pipx`](https://pipx.pypa.io/stable/) or [`uv`](https://docs.astral.sh/uv/guides/tools/#installing-tools) (recommended)

        ```sh
        pipx install "privatebin[cli]"
        ```
        ```sh
        uv tool install "privatebin[cli]"
        ```

    - With [`pip`](https://pip.pypa.io/en/stable/installation/)

        ```sh
        pip install "privatebin[cli]"
        ```

## Docs

Checkout the [quick start page](https://ravencentric.cc/privatebin/quick-start/) and the [API reference](https://ravencentric.cc/privatebin/api-reference/client/).

## License

Distributed under the [MIT](https://choosealicense.com/licenses/mit/) License. See [LICENSE](https://github.com/Ravencentric/privatebin/blob/main/LICENSE) for more information.

---

# 11. pyanilist
<br/>
<p align="center">
  <a href="https://github.com/Ravencentric/pyanilist">
    <img src="https://raw.githubusercontent.com/Ravencentric/pyanilist/main/docs/assets/logo.png" alt="Logo" width="400">
  </a>
  <p align="center">
    A Python wrapper for the AniList API.
  </p>
</p>

<div align="center">

[![PyPI - Version](https://img.shields.io/pypi/v/pyanilist?link=https%3A%2F%2Fpypi.org%2Fproject%2Fpyanilist%2F)](https://pypi.org/project/pyanilist/)
![PyPI - Python Version](https://img.shields.io/pypi/pyversions/pyanilist)
![License](https://img.shields.io/github/license/Ravencentric/pyanilist)
![PyPI - Types](https://img.shields.io/pypi/types/pyanilist)

![GitHub Workflow Status (with event)](https://img.shields.io/github/actions/workflow/status/Ravencentric/pyanilist/release.yml)
![GitHub Actions Workflow Status](https://img.shields.io/github/actions/workflow/status/ravencentric/pyanilist/tests.yml?label=tests)
[![codecov](https://codecov.io/gh/Ravencentric/pyanilist/graph/badge.svg?token=B45ODO7TEY)](https://codecov.io/gh/Ravencentric/pyanilist)

</div>

## Installation

`pyanilist` is available on [PyPI](https://pypi.org/project/pyanilist/), so you can simply use [pip](https://github.com/pypa/pip) to install it.

```sh
pip install pyanilist
```

## Usage

```py
from pyanilist import AniList

with AniList() as anilist:
    media = anilist.get_media("My Hero Academia")
    print(media.title.romaji)
    #> Boku no Hero Academia
    print(media.site_url)
    #> https://anilist.co/anime/21459
    print(media.episodes)
    #> 13
```

## Docs

Checkout the complete documentation [here](https://ravencentric.cc/pyanilist/).

## License

Distributed under the [MIT](https://choosealicense.com/licenses/mit/) License. See [LICENSE](https://github.com/Ravencentric/pyanilist/blob/main/LICENSE) for more information.

---

# 12. pynyaa
<br/>
<p align="center">
  <a href="https://github.com/Ravencentric/pynyaa">
    <img src="https://raw.githubusercontent.com/Ravencentric/pynyaa/main/docs/assets/logo.png" alt="Logo" width="400">
  </a>
  <p align="center">
    Turn nyaa.si releases into neat Python objects.
    <br/>
    <br/>
  </p>
</p>

<p align="center">
<a href="https://pypi.org/project/pynyaa/"><img src="https://img.shields.io/pypi/v/pynyaa" alt="PyPI - Version" ></a>
<img src="https://img.shields.io/pypi/pyversions/pynyaa" alt="PyPI - Python Version">
<img src="https://img.shields.io/github/license/Ravencentric/pynyaa" alt="License">
<img src="https://img.shields.io/pypi/types/pynyaa" alt="PyPI - Types">
</p>

<p align="center">
<img src="https://img.shields.io/github/actions/workflow/status/Ravencentric/pynyaa/release.yml" alt="GitHub Build Workflow Status">
<img src="https://img.shields.io/github/actions/workflow/status/ravencentric/pynyaa/tests.yml" alt="GitHub Tests Workflow Status">
<a href="https://codecov.io/gh/Ravencentric/pynyaa"><img src="https://codecov.io/gh/Ravencentric/pynyaa/graph/badge.svg?token=9LZ2I4LDYT" alt="Codecov Status"></a>
</p>

## Installation

`pynyaa` is available on [PyPI](https://pypi.org/project/pynyaa/), so you can simply use [pip](https://github.com/pypa/pip) to install it.

```sh
pip install pynyaa
```

# Docs

Checkout the [quick start page](https://ravencentric.cc/pynyaa/quick-start/) and the [API reference](https://ravencentric.cc/pynyaa/api-reference/clients/).

## License

Distributed under the [MIT](https://choosealicense.com/licenses/mit/) License. See [LICENSE](https://github.com/Ravencentric/pynyaa/blob/main/LICENSE) for more information.

---

# 13. rnzb
# rnzb

[![Tests](https://img.shields.io/github/actions/workflow/status/Ravencentric/rnzb/tests.yml?label=tests)](https://github.com/Ravencentric/rnzb/actions/workflows/tests.yml)
[![Build](https://img.shields.io/github/actions/workflow/status/Ravencentric/rnzb/release.yml?label=build)](https://github.com/Ravencentric/rnzb/actions/workflows/release.yml)
![PyPI - Types](https://img.shields.io/pypi/types/rnzb)
![License](https://img.shields.io/pypi/l/rnzb?color=success)

[![PyPI - Latest Version](https://img.shields.io/pypi/v/rnzb?color=blue)](https://pypi.org/project/rnzb)
![PyPI - Python Version](https://img.shields.io/pypi/pyversions/rnzb)
![PyPI - Implementation](https://img.shields.io/pypi/implementation/rnzb)

Python bindings to the [nzb-rs](https://crates.io/crates/nzb-rs) library - a [spec](https://sabnzbd.org/wiki/extra/nzb-spec) compliant parser for [NZB](https://en.wikipedia.org/wiki/NZB) files, written in Rust.

## Table Of Contents

- [About](#about)
- [Installation](#installation)
- [Related projects](#related-projects)
- [Performance](#performance)
- [Building from source](#building-from-source)
- [License](#license)
- [Contributing](#contributing)

## About

`rnzb.Nzb` is a drop-in replacement for [`nzb.Nzb`](https://ravencentric.cc/nzb/api-reference/parser/#nzb.Nzb).

For documentation and usage examples, refer to the [`nzb`](https://pypi.org/project/nzb) library's resources:

- [Tutorial](https://ravencentric.cc/nzb/tutorial/)
- [API Reference](https://ravencentric.cc/nzb/api-reference/parser/)

### Error handling

- `rnzb.InvalidNzbError` is named identically to `nzb.InvalidNzbError`, but it's not a drop-in replacement.
- Error messages will be largely similar to `nzb`'s, though not guaranteed to be identical in every case.
- `rnzb.InvalidNzbError` is a simpler exception (See [PyO3/pyo3#295](https://github.com/PyO3/pyo3/issues/295) for why). Its implementation is effectively:
  
  ```python
  class InvalidNzbError(Exception): pass
  ```
  
  This means that it's lacking custom attributes like `.message` found in `nzb`'s version. Code relying on such attributes on `nzb.InvalidNzbError` will require adjustment. Consider using the standard exception message (`str(e)`) to achieve the same result.
- `rnzb` will *only ever* raise explicitly documented errors for each function. Undocumented errors should be reported as bugs.

## Installation

`rnzb` is available on [PyPI](https://pypi.org/project/rnzb/), so you can simply use pip to install it.

```bash
pip install rnzb
```

## Related projects

Considering this is the fourth library for parsing a file format that almost nobody cares about and lacks a formal specification, here's an overview to help you decide:

| Project                                                  | Description                 | Parser | Meta Editor |
| -------------------------------------------------------- | --------------------------- | ------ | ----------- |
| [`nzb`](https://pypi.org/project/nzb)                    | Original Python Library     | ✅     | ✅          |
| [`nzb-rs`](https://crates.io/crates/nzb-rs)              | Rust port of `nzb`          | ✅     | ❌          |
| [`rnzb`](https://pypi.org/project/nzb)                   | Python bindings to `nzb-rs` | ✅     | ❌          |
| [`nzb-parser`](https://www.npmjs.com/package/nzb-parser) | Javascript port of `nzb`    | ✅     | ❌          |

## Performance

`rnzb` is approximately two to three times faster than [`nzb`](https://pypi.org/project/nzb/), depending on the NZB.

```console
$ hyperfine --warmup 1 --shell=none ".venv/Scripts/python.exe -I -B test_nzb.py" ".venv/Scripts/python.exe -I -B test_rnzb.py"
Benchmark 1: .venv/Scripts/python.exe -I -B test_nzb.py
  Time (mean ± σ):     368.9 ms ±   3.1 ms    [User: 196.9 ms, System: 160.9 ms]
  Range (min … max):   364.4 ms … 374.1 ms    10 runs

Benchmark 2: .venv/Scripts/python.exe -I -B test_rnzb.py
  Time (mean ± σ):     112.7 ms ±   1.2 ms    [User: 45.1 ms, System: 60.7 ms]
  Range (min … max):   111.4 ms … 116.2 ms    26 runs

Summary
  .venv/Scripts/python.exe -I -B test_rnzb.py ran
    3.27 ± 0.04 times faster than .venv/Scripts/python.exe -I -B test_nzb.py
```

## Building from source

Building from source requires the [Rust toolchain](https://rustup.rs/) and [Python 3.9+](https://www.python.org/downloads/).

- With [`uv`](https://docs.astral.sh/uv/):

  ```bash
  git clone https://github.com/Ravencentric/rnzb
  cd rnzb
  uv build
  ```

- With [`pypa/build`](https://github.com/pypa/build):

  ```bash
  git clone https://github.com/Ravencentric/rnzb
  cd rnzb
  python -m build
  ```

## License

Licensed under either of:

- Apache License, Version 2.0 ([LICENSE-APACHE](https://github.com/Ravencentric/rnzb/blob/main/LICENSE-APACHE) or <https://www.apache.org/licenses/LICENSE-2.0>)
- MIT license ([LICENSE-MIT](https://github.com/Ravencentric/rnzb/blob/main/LICENSE-MIT) or <https://opensource.org/licenses/MIT>)

at your option.

## Contributing

Contributions are welcome! Unless you explicitly state otherwise, any contribution intentionally submitted for inclusion in the work by you, as defined in the Apache-2.0 license, shall be dual licensed as above, without any additional terms or conditions.

---

# 14. seadex
<br/>
<p align="center">
  <a href="https://github.com/Ravencentric/seadex">
    <img src="https://raw.githubusercontent.com/Ravencentric/seadex/refs/heads/main/docs/assets/logo.png" alt="Logo" width="200">
  </a>
  <p align="center">
    Python wrapper for the SeaDex API.
  </p>
</p>

<div align="center">

[![PyPI - Version](https://img.shields.io/pypi/v/seadex?link=https%3A%2F%2Fpypi.org%2Fproject%2Fseadex%2F)](https://pypi.org/project/seadex/)
![PyPI - Python Version](https://img.shields.io/pypi/pyversions/seadex)
![License](https://img.shields.io/github/license/Ravencentric/seadex)
![PyPI - Types](https://img.shields.io/pypi/types/seadex)

![GitHub Workflow Status (with event)](https://img.shields.io/github/actions/workflow/status/Ravencentric/seadex/release.yml)
![GitHub Actions Workflow Status](https://img.shields.io/github/actions/workflow/status/ravencentric/seadex/tests.yml?label=tests)
[![codecov](https://codecov.io/gh/Ravencentric/seadex/graph/badge.svg?token=B45ODO7TEY)](https://codecov.io/gh/Ravencentric/seadex)

</div>

## Table Of Contents

* [About](#about)
* [Installation](#installation)
* [Docs](#docs)
* [License](#license)

## About

Python wrapper for the [SeaDex API](https://releases.moe/about/).

## Installation

`seadex` is available on [PyPI](https://pypi.org/project/seadex/), and can be installed using [pip](https://pip.pypa.io/en/stable/installation/).

1. To install the core library:

    ```sh
    pip install seadex
    ```

2. `seadex` includes optional dependencies that enable additional features. You can install these extras alongside the core library.

    - To enable the `SeaDexTorrent` class, which handles `.torrent` files:

        ```sh
        pip install "seadex[torrent]"
        ```

    - To enable the CLI:

        - With [`pipx`](https://pipx.pypa.io/stable/) or [`uv`](https://docs.astral.sh/uv/guides/tools/#installing-tools) (recommended for CLIs):

            ```sh
            pipx install "seadex[cli]"
            ```
            ```sh
            uv tool install "seadex[cli]"
            ```

        - With `pip`:

            ```sh
            pip install "seadex[cli]"
            ```

## Docs

Checkout the complete documentation [here](https://ravencentric.cc/seadex/).

## License

Distributed under the [MIT](https://choosealicense.com/licenses/mit/) License. See [LICENSE](https://github.com/Ravencentric/seadex/blob/main/LICENSE) for more information.

---

# 15. seadex-data-refinement
SeaDex Data Refinement
======================

Deployed at <https://ravencentric.github.io/seadex-data-refinement/>
---

# 16. stringenum
# stringenum

<div align="center">

[![PyPI - Version](https://img.shields.io/pypi/v/stringenum?link=https%3A%2F%2Fpypi.org%2Fproject%2Fstringenum%2F)](https://pypi.org/project/stringenum/)
![PyPI - Python Version](https://img.shields.io/pypi/pyversions/stringenum)
![License](https://img.shields.io/github/license/Ravencentric/stringenum)
![Checked with mypy](https://www.mypy-lang.org/static/mypy_badge.svg)
![Ruff](https://img.shields.io/endpoint?url=https://raw.githubusercontent.com/astral-sh/ruff/main/assets/badge/v2.json)

![GitHub Workflow Status (with event)](https://img.shields.io/github/actions/workflow/status/Ravencentric/stringenum/release.yml)
![GitHub Actions Workflow Status](https://img.shields.io/github/actions/workflow/status/ravencentric/stringenum/tests.yml?label=tests)
[![codecov](https://codecov.io/gh/Ravencentric/stringenum/graph/badge.svg?token=812Q3UZG7O)](https://codecov.io/gh/Ravencentric/stringenum)

</div>

## Table Of Contents

* [About](#about)
* [Installation](#installation)
* [Usage](#usage)
* [License](#license)

# About

A small, dependency-free library offering additional [enum.StrEnum](https://docs.python.org/3/library/enum.html#enum.StrEnum) subclasses and a backport for older Python versions.

## Installation

`stringenum` is available on [PyPI](https://pypi.org/project/stringenum/), so you can simply use [pip](https://github.com/pypa/pip) to install it.

```sh
pip install stringenum
```

# Usage

- `stringenum.StrEnum` - A backport of [`enum.StrEnum`](https://docs.python.org/3/library/enum.html#enum.StrEnum). While `StrEnum` was added in Python 3.11, version 3.12 brought changes to the `__contains__` method in `EnumType`, which also impacts `StrEnum`. `stringenum.StrEnum` includes this `__contains__` update from Python 3.12.

  - `enum.StrEnum` on Python <=3.11
    ```py
    >>> class Color(enum.StrEnum):
    ...     RED = "RED"
    ...     GREEN = "GREEN"

    >>> Color.RED in Color
    True
    >>> "RED" in Color
    Traceback (most recent call last):
      ...
    TypeError: unsupported operand type(s) for 'in': 'str' and 'EnumType'
    ```

  - `enum.StrEnum` on Python >=3.12
    ```py
    >>> class Color(enum.StrEnum):
    ...     RED = "RED"
    ...     GREEN = "GREEN"

    >>> Color.RED in Color
    True
    >>> "RED" in Color
    True
    >>> 12 in Color
    False
    ```

  - `stringenum.StrEnum` on Python >=3.9
    ```py
    >>> class Color(stringenum.StrEnum):
    ...     RED = "RED"
    ...     GREEN = "GREEN"

    >>> Color.RED in Color
    True
    >>> "RED" in Color
    True
    >>> 12 in Color
    False
    ```

- `stringenum.DuplicateFreeStrEnum` - A subclass of `StrEnum` that ensures all members have unique values and names, raising a `ValueError` if duplicates are found.

    ```py
    >>> class Fruits(DuplicateFreeStrEnum):
    ...     APPLE = "apple"
    ...     BANANA = "banana"
    ...     ORANGE = "apple"
    ...
    Traceback (most recent call last):
      ...
    ValueError: Duplicate values are not allowed in Fruits: <Fruits.ORANGE: 'apple'>
    ```

- `stringenum.CaseInsensitiveStrEnum` - A subclass of `DuplicateFreeStrEnum` that supports case-insensitive lookup.

    ```py
    >>> class Pet(CaseInsensitiveStrEnum):
    ...     CAT = "meow"
    ...     DOG = "bark"

    >>> Pet("Meow")
    <Pet.CAT: 'meow'>

    >>> Pet("BARK")     
    <Pet.DOG: 'bark'>

    >>> Pet["Cat"]
    <Pet.CAT: 'meow'>

    >>> Pet["dog"] 
    <Pet.DOG: 'bark'>
    ```

- `stringenum.DoubleSidedStrEnum` - A subclass of `DuplicateFreeStrEnum` that supports double-sided lookup, allowing both member values and member names to be used for lookups.

    ```py
    >>> class Status(DoubleSidedStrEnum):
    ...     PENDING = "waiting"
    ...     REJECTED = "denied"

    >>> Status("PENDING")
    <Status.PENDING: 'waiting'>

    >>> Status("waiting")
    <Status.PENDING: 'waiting'>

    >>> Status["REJECTED"]
    <Status.REJECTED: 'denied'>

    >>> Status["denied"]
    <Status.REJECTED: 'denied'>
    ```

- `stringenum.DoubleSidedCaseInsensitiveStrEnum` - A subclass of `DuplicateFreeStrEnum` that supports case-insenitive double-sided lookup, allowing both member values and member names to be used for lookups.

    ```py
    >>> class Status(DoubleSidedCaseInsensitiveStrEnum):
    ...     PENDING = "waiting"
    ...     REJECTED = "denied"

    >>> Status("pending")
    <Status.PENDING: 'waiting'>

    >>> Status("Waiting")
    <Status.PENDING: 'waiting'>

    >>> Status["Rejected"]
    <Status.REJECTED: 'denied'>

    >>> Status["DenieD"]
    <Status.REJECTED: 'denied'>
    ```

## License

Distributed under the [MIT](https://choosealicense.com/licenses/mit/) License. See [LICENSE](https://github.com/Ravencentric/stringenum/blob/main/LICENSE) for more information.

---
