# What Navidrome Is

**Scope:** the mental model — a music
server you run yourself, which reads the
songs you already own and streams them to
your browser and your phone — plus what
it is not, and the lab this guide's
claims rest on.

## The one-paragraph version

**Navidrome** is open-source software
that turns a folder of music files on a
computer in your house into a streaming
service for an audience of you. It reads
the **tags** inside your MP3, FLAC, and
other audio files — artist, album, title,
track number — builds a library out of
them, and serves it back through a web
page that looks like a small, honest
streaming service: browse by artist or
album, search, press play. The same
server also speaks the **Subsonic API**,
a long-lived protocol that dozens of
phone and desktop apps already know, so
the music follows you into apps that were
never written for Navidrome at all. Your
files stay where they are, in ordinary
folders, in ordinary formats. Navidrome
never moves them, renames them, or locks
them into anything.

That is the whole idea. The rest of this
guide is the fine print: what the one
program keeps in its database, why tags
matter more than folders, how extra
listeners get fenced in, and the backups
that decide whether a disk failure is an
afternoon or a catastrophe.

## The shape of the thing

Three pieces of the experience are worth
naming before any machinery:

- **The library is read, not imported.**
  Point Navidrome at your music folder
  and it scans it, much as a librarian
  walks the shelves and writes a
  catalogue. The catalogue — the
  **database** — is Navidrome's. The
  books stay on the shelves. Delete
  Navidrome tomorrow and your music
  collection is exactly where it always
  was, untouched.
- **Tags are the truth.** Navidrome
  believes the labels inside each file,
  not the folder it sits in or the name
  it carries. A beautifully organised
  folder of badly tagged files becomes a
  messy library; a heap of files with
  good tags becomes a tidy one. Chapter 5
  is devoted to this, because it is the
  one surprise every new user meets.
- **One library, several listeners.**
  A household runs one Navidrome. Each
  person gets their own account, their
  own playlists, favourites, and play
  counts — but the library itself is
  shared. There is no notion of "her
  songs" and "his songs" inside one
  server; chapter 9 shows exactly where
  the fences stand, measured, not
  assumed.

## What it is not

Four confusions worth clearing away
early:

- It is **not a streaming subscription**.
  Navidrome contains no music and sells
  none. It streams *your* files. If your
  collection is thin, Navidrome will be
  thin in exactly the same places.
- It is **not a tag editor or a ripper**.
  It reads your files as they are. It
  will not fix spelling, fetch missing
  track numbers into the files, or rip a
  CD. Chapter 5 names the tools that do
  those jobs; Navidrome is the stage,
  not the workshop.
- It is **not a sync tool**. It does not
  copy your library to your phone for
  offline safekeeping as its main act
  (some Subsonic apps can download for
  offline play — chapter 7 — but that is
  the app spending your server's music,
  not a backup). Your collection's safety
  is a separate job: chapter 11.
- It is **not silent toward the
  internet**. Out of the box, Navidrome
  asks outside services about your
  artists — biographies and pictures from
  Deezer, Last.fm, and ListenBrainz — and
  sends anonymous usage statistics to the
  project. That is on by default, it is
  visible in the log, and chapter 10
  shows the one option that turns all of
  it off, if that trade isn't yours.

## Versions and the lab

This guide was built against **Navidrome
v0.64.2**, the project's current release
on the day of the build, downloaded from
the project's own release page and run
directly — no containers, no package
manager, one binary. Onto it the lab put
a small specimen library: seven files
generated on the build machine with
`ffmpeg` — pure tones wearing invented
artists, albums, and proper tags, plus
one file with no tags at all and one
file that is not audio at all, to watch
the scanner cope. No real recording, and
no person's music, was involved
anywhere. The lab then exercised both of
the server's public faces — the modern
**native API** its web page uses, and the
Subsonic API the apps use — through
first setup, scanning, streaming,
transcoding, playlists, a second user,
a backup restored into a fresh folder,
and an upgrade from the previous release
to this one. The transcripts live in
`navidrome-lab/transcripts/`; `criteria.md`
keeps the ledger of what was run and
what was only read.

Check: you should now be able to say
where the music lives, what Navidrome
adds on top of it (a catalogue and a
stream, nothing more), and why fixing a
wrong artist name will send you to your
tags and not to Navidrome. If that is
clear, chapter 2 opens the box.
