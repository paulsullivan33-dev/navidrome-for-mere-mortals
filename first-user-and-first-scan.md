# First User and First Scan

**Scope:** the first account (which is
the admin, by definition), what the
scanner made of the lab's seven files,
and reading the library the way the
server sees it — including the two
specimens that came out wrong on
purpose.

## The first account is the admin

Fresh Navidrome has no users at all, and
its doors are locked in both directions:
the Subsonic API answers a stranger with
error 40, "Wrong username or password,"
and the native API with a plain 401
(transcript t00). The first screen of
the web interface is therefore not a
login but an invitation: **create the
first user**. Behind the form, the page
calls a one-time endpoint —
`POST /auth/createAdmin` — which only
works while no users exist.

Two choices on that screen deserve a
slow moment:

- **The username and password are real
  credentials.** This account is the
  admin: it will manage the server and
  every other user. Pick a password you
  would give a house key, and keep it in
  your password manager. Losing it is
  recoverable — the binary's own
  `navidrome user` commands administer
  accounts directly on the database,
  and the lab ran `navidrome user list`
  against its live server without
  disturbing it (transcript t14) — but
  there is no
  "email me a reset link" on a server
  that is only yours.
- **You will make a second, ordinary
  account for daily listening if anyone
  else uses the server** — and arguably
  even if they don't. Chapter 9 shows
  why: the admin's powers are wide, and
  there is no reason your phone's music
  app should hold them.

Transcript t01 is the lab's first
minute: the endpoint answers with the
new user, `isAdmin: true`, and a token;
an ordinary login then returns the same
shape. After this moment the door locks
behind you — `createAdmin` never works
again, and strangers go back to error
40.

## The first scan already happened

You may not need to ask for a scan at
all. On its first start, Navidrome scans
the music folder on its own (the lab's
log: `Scanner: Finished scanning all
libraries`, two and a half seconds for
seven files), and its **watcher** keeps
scanning as files arrive. One detail
from that first log is worth knowing
before it puzzles you: if a scan happens
before any admin exists, `.m3u`
playlists are held back — "Playlists
will not be imported, as there are no
admin users yet" — because an imported
playlist must belong to somebody. They
import themselves once the first admin
exists. Chapter 8 tells the playlist
story in full.

## Reading the library back

The honest way to meet your new server
is to ask it what it thinks it has. The
web page shows it prettily; the lab
asked through the native API, the same
interface the page uses, and transcript
t02 is the whole reply. The specimen
library came back as:

| Artist | Albums | Songs |
|---|---|---|
| The Lab Tones | First Frequencies (2024) | 3 |
| Ada Waves | Harbour Lights (2025) | 2 |
| [Unknown Artist] | [Unknown Album] | 2 |

Every tagged file landed exactly where
its tags said: right titles, right track
numbers (1, 2, 3 in order), the genre
("Specimen," the lab's little joke)
carried through, formats and durations
correct. That is the scanner doing
precisely what chapter 2 promised —
reading labels, believing them.

The third row is the lesson. The
untagged file had no artist to read, so
it fell back to its filename for a
title — `mystery-tone` — and was filed
under **[Unknown Artist]** and
**[Unknown Album]**, the library's lost
property office. And the file that was
not audio at all — a text file wearing
an `.mp3` name — was catalogued right
beside it as a "song" of zero seconds
and zero bitrate, titled `broken`.
Navidrome does not deeply interrogate
every file at scan time; it catalogues
what it finds, and the fraud is only
exposed when somebody presses play
(chapter 16 picks this up: in
transcript t04 the "song" streams its
33 bytes of text back, HTTP 200, and
any real player will simply fail on it).

**Check:** your library's artists and
albums match what your tags claim — not
what your folders claim. Where they
disagree, believe the server's version
of the story and go and look at the
tags; that is chapter 5, and it is the
most useful chapter in this guide for a
real collection.

## A note on the outside calls

Within seconds of the lab's first scan,
its log carried warnings like `Artist
not found in deezer` (transcript t10).
That is the default behaviour doing its
job: Navidrome asked Deezer — first of
its default agents — for biographies and
pictures of the artists, and invented
artists are, unsurprisingly, not in
Deezer. Your real artists mostly will
be, and the results fill the web page's
artist screens. If the idea of your
server phoning out about your taste
doesn't suit you, chapter 10 has the
switch. Nothing breaks either way; the
music never leaves the house — only the
artists' names go asking.
