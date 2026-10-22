# The Shape of It

**Scope:** one program and three
folders — the binary, the music folder,
the data folder — and the fourth thing
worth knowing about, the cache, which
you are allowed to forget.

## One program

Navidrome is a single executable file.
There is no database server to install,
no web server to configure, no runtime
to babysit: the one binary contains the
scanner, the streaming engine, the web
page, and both APIs. Chapter 3's whole
install is, at heart, "download the
file, run the file."

That simplicity cuts both ways, and the
guide will say so where it matters: one
program means one process to watch, one
thing to upgrade (chapter 12 is a file
swap), and one point of failure (chapter
15, Limits, is blunt about it).

## Three folders

Everything Navidrome touches lives in
three places, and most later confusion
is one of these wearing another's name:

| Folder | Holds | If it vanished |
|---|---|---|
| The **music folder** (`MusicFolder`) | Your audio files, exactly as you keep them | The actual loss. Navidrome can rebuild everything except this. |
| The **data folder** (`DataFolder`) | The database — users, passwords, playlists, stars, play counts, the catalogue the scanner built — plus caches and any plugins | Painful amnesia. The music survives; every account, playlist, and favourite is gone. Chapter 11 exists because of this row. |
| The **config file** (`navidrome.toml`) | Your choices: where the folders are, which port, the handful of switches from chapter 10 | Mild. It is a dozen lines; you could rewrite it from memory after reading this guide. |

The database is a single file,
`navidrome.db`, in the data folder —
an ordinary **SQLite** database. The lab
watched it sit at about two-thirds of a
megabyte for a seven-track library; it
grows with the collection and with every
playlist and play count you add, but for
any household library it stays small
next to the music itself. While the
server runs you will also see
`navidrome.db-wal` and `navidrome.db-shm`
beside it — SQLite's working papers,
which fold back into the main file when
the server stops cleanly. Remember this
when chapter 11 talks about backups:
copying the database of a *running*
server means minding all three files,
or using the tool that does it properly.

## The fourth thing: the cache

Inside the data folder, Navidrome keeps
a `cache` folder with two tenants:

- **Transcoding cache** (default size
  cap 100 MB): the converted copies it
  makes when a listener's app asks for a
  smaller stream than the original file
  (chapter 7). Bounded, self-cleaning,
  and entirely disposable.
- **Image cache** (default cap 100 MB):
  resized album covers and artist
  pictures, so your phone isn't sent a
  4 MB scan of a CD booklet to render as
  a thumbnail.

Neither cache is backed up, mourned, or
explained again. If the cache folder is
deleted, the only symptom is a slower
first day while it refills.

## How a song travels

One journey, end to end, with every stop
this guide will revisit:

1. You drop a file into the music
   folder. Navidrome's **watcher** —
   a part of the program that keeps an
   eye on the folder — notices, usually
   within seconds. (The lab's playlist
   file was imported about a second
   after it landed; transcript t12.)
2. The **scanner** reads the file's
   tags and writes the song, its artist,
   and its album into the database.
   Files whose tags disagree with their
   neighbours become their own artists
   and albums — chapter 5's whole story.
3. Someone presses play — in the web
   page, or in an app speaking Subsonic.
   The server finds the song in the
   database, opens the real file in the
   music folder, and streams it.
4. If the listener asked for less than
   the original — an app on mobile data
   asking for 48 kbps, say — **ffmpeg**
   converts the stream on the way out.
   ffmpeg is the one outside tool
   Navidrome truly depends on; the
   install chapter makes sure it is
   present. The lab's proof is
   transcript t05: a FLAC went in, a
   48 kbps Opus stream came out.
5. The play is written back to the
   database: play counts, "recently
   played," and the raw material for
   the server's statistics are all just
   rows in `navidrome.db`.

Check: given a symptom, you should be
able to guess its folder. Wrong artist
names? The music folder — the tags live
there. A playlist gone? The data folder.
The server on the wrong port? The
config file. Chapter 3 puts the program
and its three folders in place.
