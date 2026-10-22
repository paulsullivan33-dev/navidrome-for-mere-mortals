# Writing criteria

This guide was written against the
series' standing nine-point standard.
Each criterion is stated with what it
demanded of a **self-hosted music
server** guide, and — where the standard
asks for proof — what the lab notebook
says.

## The nine points

1. **Real installation, real evidence.**
   The guide's system is installed and
   run for real while the guide is
   written, and the facts in it come
   from that run — not from the
   project's brochure. Where a claim
   could not be exercised, it is
   labelled documented rather than
   presented as observed.
2. **No placeholders.** No "insert your
   X" left standing in commands a
   beginner is told to run, and no
   chapter that gestures at a procedure
   instead of giving it.
3. **A clear path from the start.** A
   reader can go from this README to a
   working first listen in one pass,
   meeting each idea before the first
   procedure that needs it.
4. **Stands alone.** Terms are defined
   where they are first load-bearing
   (and again in the glossary); no
   other guide in the series is
   required reading.
5. **Honest about failure.** Errors are
   shown as they actually appeared,
   including the ones the lab caused on
   purpose and the one it found by
   accident, and the fix tested the
   same way.
6. **Names its own limits.** What the
   software won't do gets a chapter,
   not a footnote — including the
   limits of this lab itself.
7. **Technically accurate.** Versions,
   defaults, API responses, and log
   lines quoted in the guide were
   produced by the version they are
   attributed to, and checked against
   the running server where the guide
   claims they were.
8. **Fictional fixtures only.** Every
   library, user, and organisation in
   the guide is invented for the guide.
   No real person's collection,
   accounts, or taste appears anywhere
   in it.
9. **A working, re-runnable lab.** The
   verification can be repeated from
   the transcripts and lab notes by a
   reader with the same release and an
   hour to spare.

## The lab, and the ledger

The lab ran **Navidrome v0.64.2**
(linux_amd64 release build, SHA-256
verified against the project's
published `navidrome_checksums.txt`)
as a plain binary in a sandbox,
against a specimen library of seven
files generated on the machine: three
tagged MP3 tones and two tagged FLAC
tones (pure tones wearing invented
artists and full tags, written by
`ffmpeg`), one MP3 with no tags, and
one text file named `.mp3`. Two
invented users — Ruth (the first
account, therefore admin) and Kenji
Mori (a listener) — exercised the
server exactly as the chapters
describe. The transcripts are in
`navidrome-lab/transcripts/` and the
build notes in
`examples/lab-notes.txt`.

**Proved live, on the bench:**

- First-run state: an unconfigured
  server refuses both APIs (Subsonic
  error 40; native API 401), and the
  first account is created through
  `POST /auth/createAdmin`, after which
  ordinary login succeeds (t00, t01).
- The scan of the specimen library:
  artists, albums, track order, genre,
  formats, and durations matched the
  tags exactly; the untagged file fell
  back to filename and [Unknown
  Artist]; the non-audio file was
  catalogued as a zero-duration,
  zero-bitrate track (t02).
- Subsonic salt-and-token auth by hand:
  ping, getUser roles, search3 across
  artists/albums/songs (t03).
- Streaming integrity: an unconverted
  FLAC stream is SHA-256-identical to
  the file on disk; the fraudulent file
  streams its text bytes back with
  HTTP 200 (t04).
- Transcoding: a FLAC requested at
  `maxBitRate=48` arrived as Opus at
  about 48 kbps, with the server log
  confirming `transcoding=true
  format=opus bitRate=48` (t05).
- A playlist created over Subsonic is
  visible through the native API, born
  with `"public": false` (t06).
- A second, non-admin user: created by
  the admin, signs in, is fenced from
  the user list (403) and from creating
  users (403), sees none of the admin's
  playlists — including by direct ID
  (Subsonic error 70, "playlist not
  found") — while streaming and
  searching the shared library freely
  (t07, t08).
- Stars are per-user: Ruth's starred
  song is absent from Kenji's starred
  list (t09).
- Defaults, observed: the scanner's
  external metadata lookups (Deezer
  "artist not found" for the invented
  artists) and the boot log's
  announcements (routes, caches, login
  rate limit, ffmpeg found) (t00, t10).
- A music folder that does not exist
  does not stop the server: it serves
  an empty library and logs the path
  errors (t10).
- Backup and restore, rehearsed end to
  end: `backup create` produced a
  timestamped database file; `backup
  restore` into an empty data folder
  failed with "No existing database";
  after a first boot created a
  database, restore with `--force`
  succeeded, and the restored instance
  kept both users' passwords, the
  playlist (same ID), the star, and the
  library (t11).
- `.m3u` auto-import: a playlist file
  dropped into the music folder was
  imported by the watcher within
  seconds as a synced, read-only
  playlist owned by the admin (t12).
- Upgrade, rehearsed: v0.63.2 created
  a database and an admin; the v0.64.2
  binary started over the same data
  folder, logged its goose migration
  to the new schema version, and served
  the same admin and library at the new
  version (t13).
- `navidrome user list` runs against
  the live server and reports both
  accounts (t14). `navidrome doctor`
  reported the lab database's integrity
  and foreign-key checks clean.

**Documented only — read, not run:**

- The **Docker install** and the
  `.deb`/`.rpm`/NAS package routes:
  described from the project's
  installation page (chapter 3). The
  binary route is the one this guide
  verifies.
- The **Windows and macOS builds**:
  published by the project; not
  available on this bench.
- **Clicking through the web
  interface**: the lab exercised the
  native API the interface is built on
  and fetched the interface itself
  (it answers and identifies its
  version); it did not drive a browser
  through the screens, and chapter 6
  says so.
- **Third-party Subsonic apps** on
  phones and desktops: the protocol
  they speak was exercised directly and
  completely; no app was installed, so
  chapter 7 lists clients as a map,
  not a review.
- **Public share links**: the routes
  are mounted and the feature exists;
  the lab did not mint one, for the
  reasons chapter 8 gives.
- **Scrobbling** (Last.fm/ListenBrainz)
  and the **jukebox and podcast**
  features: present in the software,
  named where relevant, not exercised.
- **Performance and scale** beyond a
  seven-song library: chapter 15 bounds
  its claims accordingly.

Two surprises earned their keep in the
chapters. The scanner's trust: a text
file in an MP3 costume is catalogued
and even streamed without complaint —
the catalogue believes names and tags,
not content (chapters 4, 5, 16). And
the restore door's one-way hinge: a
backup cannot create a database in an
empty folder, only replace one — a
fact you want from a rehearsal, not
from a disaster (chapter 11).
