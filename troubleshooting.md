# Troubleshooting

**Scope:** the failures this lab
actually produced, plus the standard
ones the project's own pages answer —
each as symptom, cause, and the check
that tells them apart. The order is
the order you're likely to meet them.

## The discipline first

Two tools answer most questions before
any guessing starts: the **server log**
(it narrates scans, streams, transcodes,
and lookups; chapter 10's `debug` level
makes it verbose) and **`ffprobe`**,
for asking a file what it really is.
A third, `navidrome doctor`, checks the
database's own health — the lab ran it
on its working server and got its
integrity and foreign-key checks back
clean, so a dirty result later would
mean something had genuinely changed.

## The library is empty, but the server runs

**Seen in the lab** (transcript t10).
Point a Navidrome at a music folder
that doesn't exist and it does *not*
refuse to start: it serves a clean,
empty library and confines its distress
to the log — `Error resolving path`,
`Watcher error`, `Scanner: Error
creating scan context`. **Cause:**
`MusicFolder` wrong, a drive unmounted,
a path typo. **Check:** the log lines
above, then the path in the config
against the path on disk, letter by
letter. This failure's politeness is
its trap: everything "works," so people
debug the scanner for an hour before
reading three lines of log.

## A file shows up but won't play

**Seen in the lab** (transcripts t02,
t04). The specimen `broken.mp3` — a
text file with an audio costume —
scanned in as a zero-second, zero-
bitrate "song," and streaming it
returned its text bytes with HTTP 200.
A real player fails on it, of course.
**Cause:** the file is corrupt, or is
not audio at all; the scanner
catalogues by name and tags, and does
not audition every file. **Check:**
`ffprobe the-file.mp3` — if ffprobe
can't find audio in it, neither can
anyone. Also check the catalogue's
tells: duration 0 and bitRate 0 on a
file that should have both is the
catalogue raising its hand. **Fix:**
replace or delete the file; no server
setting is involved.

## Songs land under [Unknown Artist]

**Cause, nine times in ten:** the tags
(chapter 5). The file has no artist
tag, or the album artist is spelled
three ways across the album. **Check:**
`ffprobe` the file and compare with
what the library shows — the library
is reading the same tags. **Fix:** in
the files, with a tag tool, then a
rescan. Do not look for a "correct
this artist" button in Navidrome; there
isn't one, on purpose.

## Playlists from .m3u files don't appear

**Checks, in order:** Is the file in
(or under) the music folder? Is
`AutoImportPlaylists` still on
(chapter 10)? Did the first admin exist
when it landed — imports wait for an
owner (chapter 4), and arrive by
themselves shortly after the admin
does. Do the paths inside resolve? They
are read relative to the music folder
(chapter 8); a playlist written on
another machine with absolute or
Windows-shaped paths matches nothing,
and unmatched lines are skipped
silently. Count the playlist's songs
against the file's lines — the gap is
your list of suspects.

## Apps can't connect, but the browser can

**Cause, in the order the lab would
check:** the address (the app wants
the server's LAN address and port —
`http://192.168.x.y:4533` shape — not
`localhost`, which is the phone
itself); the password (remember
chapter 7: changing it evicts every
app's stored token at once); and the
app's own server-type setting, since
some clients distinguish Subsonic from
other back ends. **Check:** the same
handshake the lab ran — any machine
with `curl` can reproduce transcript
t03's `ping`. If ping answers `ok` with
those credentials, the server is
innocent and the app is the
conversation to have.

## A listener is locked out right after sign-in fails a few times

**Seen in the boot log:** logins are
rate-limited — five attempts per
twenty seconds per address. A phone
app retrying a stale password will
trip it cheerfully and keep tripping
it. **Fix:** fix the password in the
app first, then wait the limiter out.
Not a fault; a fence doing its job
(chapter 10).

## Streaming stutters or transcoding eats the machine

**Checks:** how many listeners, and
how many are transcoding right now —
the log says `transcoding=true` per
stream (transcript t05), and the
now-playing view corroborates. A
conversion is ffmpeg working as
designed; a small machine serving six
simultaneous conversions is simply
outnumbered. **Fixes:** lower the
apps' mobile quality (chapter 13,
recipe 5); pre-warm the transcoding
cache by playing the popular albums
once at the capped setting; or accept
direct play (the file's own bitrate)
where the network allows. If ffmpeg
is *missing* rather than busy, the
boot log's "Found ffmpeg" line will
be absent and conversions will fail
outright — reinstall it (chapter 3).

## The restore refuses: "No existing database"

**Seen in the lab** (transcript t11).
`backup restore` replaces a database;
it won't create one in an empty data
folder. **Fix:** start the new instance
once so it makes its own database,
stop it, restore with `--force`.
Chapter 11 has the full rehearsal.

**Check (the chapter's whole method):**
for any symptom above, you should be
able to say which of the three folders
or two tools holds the answer before
touching anything. Symptom → log →
file → config, in that order, solves
nearly everything this server can do
to you — because nearly everything it
does, it writes down.
