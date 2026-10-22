# Glossary

**Scope:** the words this guide used
as if they were ordinary — because in
this corner of computing they are —
each defined once, in this guide's own
terms.

- **Album artist** — the tag that files
  an album under one artist even when
  individual tracks name guests.
  Chapter 5's most important tag.
- **API** — a machine-facing doorway:
  the set of requests a program will
  answer. Navidrome has two, the
  **native API** under `/api` (its web
  page's doorway) and the **Subsonic
  API** under `/rest`.
- **Cache** — kept copies of expensive
  work: converted streams and resized
  artwork, in the data folder, capped
  and disposable.
- **Data folder** (`DataFolder`) — where
  the database, caches, and plugins
  live. The folder chapter 11 backs up.
- **ffmpeg** — the universal audio/video
  converter. Navidrome's one outside
  dependency, used for transcoding.
- **FLAC** — a lossless audio format:
  the file *is* the CD's sound, bit for
  bit, at roughly half a WAV's size.
  The connoisseur's default and the
  transcoder's favourite input.
- **ID3** — the tag format inside MP3
  files; other formats have their own
  (Vorbis comments in FLAC), but "tags"
  in this guide means all of them.
- **Metadata agents** — the outside
  services (Deezer, Last.fm,
  ListenBrainz) Navidrome asks for
  artist biographies and pictures.
  Chapter 10's switch governs them.
- **Migration** — the database being
  carried to a newer schema by a newer
  program, automatically, at upgrade
  time (chapter 12).
- **Music folder** (`MusicFolder`) — your
  audio files, as you keep them. Read
  by Navidrome, never written.
- **Opus** — a modern, efficient lossy
  format; Navidrome's default when a
  stream must be shrunk (chapter 7).
- **Playlist** — an ordered list of
  song IDs with a name; made inside the
  server or imported from an `.m3u`
  file. *Synced* playlists are the
  file-owned kind (chapter 8).
- **Rescan / scan** — the scanner
  re-reading files into the catalogue:
  *quick* for changes, *full* for
  everything, after bulk tag surgery.
- **Salt and token** — Subsonic's
  sign-in arithmetic: a random salt, and
  a token hashed from password + salt,
  so the password itself never travels.
- **Scrobbling** — reporting what you
  play to a listening-history service
  (Last.fm, ListenBrainz). Optional,
  per-user, documented rather than run
  in this guide.
- **Share** — a public, account-free
  link to stream a song, album, or
  playlist. Chapter 8's documented
  feature; handle like a spare key.
- **Star** — a favourite mark (the web
  page draws it as a heart). Per-user,
  always (chapter 8).
- **Subsonic** — the 2000s music server
  whose API outlived it and became the
  protocol a generation of apps speak.
  Navidrome is its modern heir.
- **Tag** — a labelled fact stored
  *inside* an audio file: artist,
  album, title, track, year, genre.
  The library is built from these and
  nothing else (chapter 5).
- **Transcoding** — converting a stream
  while it plays, to a smaller bitrate
  or different format than the file on
  disk, at the listener's request.
- **Watcher** — the part of the scanner
  that notices files arriving or
  changing in the music folder, within
  seconds, without being asked.

**Check:** if a word in an earlier
chapter wobbled for you, reread its
sentence with this definition in hand —
the chapters were written against these
meanings, and hold together under them.
