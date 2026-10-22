# Navidrome for Mere Mortals

Your music, on your machine, streamed
like it means it.

Streaming taught a generation to rent
its record collection: the albums
arrive and vanish at a stranger's
commercial convenience, the play counts
and playlists live in someone else's
database, and the day the licensing
wind changes, the song you loved is a
grey rectangle. Meanwhile the files —
the ones you ripped, bought, and
hoarded across three laptops and a
backup drive — sit in a folder doing
nothing, already paid for, already
yours.

Navidrome is the small, stubborn answer
to that arrangement: one program, run
on a machine in your house, that reads
the tags in your files, builds a
library out of them, and streams them
to your browser and your phone with
the manners of a real streaming
service — search, playlists, favourites,
per-person accounts — while the music
itself never leaves the shelf it was
already on. It also speaks the Subsonic
protocol, so a decade's worth of music
apps you've never heard of can suddenly
play your collection like they were
born to it.

This guide builds one from nothing: the
official binary, downloaded and
checksum-checked; a first admin; a
first scan of a small specimen library
(seven generated tones in invented
artists' names — the lab's claims are
measured, and its failures, including a
text file that passed itself off as a
song, are shown rather than tidied
away); streaming proved byte-for-byte
against the files on disk; transcoding
when a phone asks for less; a second
listener fenced to exactly what a
listener should reach; a backup
restored into a fresh folder and
interrogated; and an upgrade performed
the way the program intends — swap the
file, watch the database lift itself.
Whatever can be checked in the guide
was checked against the real software;
whatever could not be is labelled
documented, in the chapters and in the
ledger that `criteria.md` keeps.

## What this guide covers

- [What Navidrome is](what-navidrome-is.md) — the mental model, and the confusions to retire early.
- [The shape of it](the-shape-of-it.md) — one program, three folders, and a cache you're allowed to forget.
- [Installing the binary](installing-the-binary.md) — the release, the checksum, ffmpeg, three lines of config, a standing service.
- [First user and first scan](first-user-and-first-scan.md) — the admin who is first by definition, and the library the scanner reports back.
- [Tags and folders](tags-and-folders.md) — the belief system of the whole library, and the tools that repair it.
- [Listening in the browser](listening-in-the-browser.md) — the web interface, its landmarks, and how this guide verified it.
- [Listening with apps](listening-with-apps.md) — the Subsonic door: salt-and-token sign-in, honest streams, transcoding, the client map.
- [Playlists, stars, and shares](playlists-stars-and-shares.md) — lists made inside, lists imported as files, favourites that are provably personal.
- [Users and permissions](users-and-permissions.md) — the second account, and the fences measured rather than assumed.
- [Configuration](configuration.md) — where settings live, the privacy switch, and the defaults that explain everything else.
- [Backing up](backing-up.md) — the three things there are to lose, the built-in backup, and a restore rehearsed for real.
- [Upgrading](upgrading.md) — swap the file, watch the migration, know the way back.
- [Recipes](recipes.md) — six ordinary jobs: new albums, household queues, plane music, a second collection, the data diet, the [Unknown] spring-clean.
- [The neighbours](the-neighbours.md) — folders and players, clouds, and the other servers, fairly described.
- [Limits](limits.md) — the shared library, the unsheltered street door, and one process's ceiling.
- [Troubleshooting](troubleshooting.md) — the empty library that isn't broken, the song that isn't a song, and the rest, in the order you'll meet them.
- [Glossary](glossary.md) — every term, defined once, in this guide's own usage.
- [Writing criteria](criteria.md) — the nine-point standard and the verification ledger for this guide.

## What it doesn't cover

Playing other people's music (there is
none in Navidrome, and none is coming),
ripping CDs, repairing tags inside the
server (chapter 5 names the tools that
do it, outside), running Navidrome as a
public internet service (chapter 15
explains why the raw port stays home,
and what the grown-up routes are), and
the client apps' inner lives (chapter 7
maps them; the protocol is the part
that lasts).

## How to use this guide

Read chapters 1–2 for the model, then
3–5 with a machine in front of you:
install, first user, first scan, tags.
Chapters 6–9 are the listening life,
10–12 the keeping of the thing, and
13–16 for when you want more — or when
it misbehaves. The `examples/` folder
holds the config, the service unit,
the compose file for the documented
Docker route, the command cheat sheet,
and the lab notes; `navidrome-lab/
transcripts/` holds the raw evidence
the chapters cite as t00–t14.
