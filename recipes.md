# Recipes

**Scope:** six small jobs a Navidrome
owner actually does in the first year,
each one a recipe: what it's for, the
moves, and the chapter that explains
the machinery underneath.

## 1. Add a new album to the library

**For:** the ordinary, happy case.
Drop the album's folder into the music
folder, in chapter 5's layout, with its
tags already right (check them in a tag
tool first if the source is new to
you). Wait a few seconds — the watcher
scans it in — and look in the browser
under the artist. If it lands under
[Unknown Artist] or splits itself in
two, the tags are the culprit; fix
them in the files and rescan
(chapter 5).

## 2. A whole-household evening queue

**For:** one playlist several people
add to. Make the playlist in the web
page, then flip its record to public:
a *public* playlist is visible to other
users of your server (chapter 9's fence
keeps *private* ones invisible — the
lab verified private by ID), so a
household list is a deliberate choice,
not a leak. Each listener adds from
their own account; the play counts and
stars they earn stay their own
(chapter 8). For a fixed, curated
list instead, write it as an `.m3u` in
the music folder and let it import as
read-only — nobody's thumb can
rearrange it by accident (chapter 8).

## 3. Music on a plane

**For:** listening away from the
server entirely. In a Subsonic app
that supports downloads (chapter 7's
two questions), pin the albums before
you leave, on Wi-Fi. Playback then
never touches the server — which is
also why your server being offline at
home doesn't matter mid-flight. What
the app cannot do is fetch *new* music
while away; treat the download list
like packing, done the night before.

## 4. A second collection on the same box

**For:** the case chapter 9 closed —
genuinely separate libraries (a
children's server, say, with its own
contents). Run a second Navidrome:
another binary copy is unnecessary —
the same binary, a second config file
(pointing at its own music folder and
its own data folder, with `Port =
4534`), a second service unit. The lab ran two
instances side by side on ports 4533
and 4534 during the restore rehearsal
(transcript t11) without either
noticing the other. Two libraries, two
admins, zero cross-talk; the cost is
two backups to keep (chapter 11).

## 5. The phone-friendly data diet

**For:** a listener whose listening is
mostly mobile data. In their app, set
the mobile streaming quality low —
48–96 kbps (chapter 7). The server
converts FLAC to Opus at the requested
ceiling on the fly and caches the
result, so a much-loved album is
converted once, ever, per setting.
Library-quality listening at home stays
untouched: Wi-Fi quality is a separate
knob, in the same app, on the same
account.

## 6. Spring-clean the [Unknown] shelf

**For:** the lost-property office of
chapter 4, once it has tenants. List
them in the browser (the unknown
artist and album pages), then work
through the files with `ffprobe` in
one hand and a tag tool in the other:
add artist, album artist, album,
title, and track number; rescan.
Delete the true frauds — the lab's
`broken.mp3` was a text file wearing a
costume, and no tagger can help a file
that isn't music (chapter 16 has its
autopsy). An [Unknown] shelf at zero
is the sign of a healthy library, and
it takes an afternoon at most per
hundred files.

**Check:** every recipe above is a
combination of chapters you've already
read — nothing here introduced a new
mechanism. If one of them felt like it
did, that's the chapter to reread; the
recipes are the index of your own
competence, which is what a recipe
chapter is for.
