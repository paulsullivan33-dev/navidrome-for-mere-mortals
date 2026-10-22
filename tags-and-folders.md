# Tags and Folders

**Scope:** the one idea the whole
library stands on — Navidrome believes
tags, not folders — which tags matter,
the layout that keeps humans and the
server both happy, and the tools that
fix tags when they're wrong.

## Believe the tags

Chapter 4's specimen library is the
whole argument in miniature. Two of its
seven files had no usable tags, and the
server didn't guess from their folder
(they sat in a folder honestly named
`Untagged`, which helped not at all),
didn't parse their filenames for an
artist, and didn't shrug — it filed
them under **[Unknown Artist]** and
built the rest of the library from
labels alone. Real collections meet the
same fate at scale: the album scattered
across three artist spellings, the
compilation shredded into one-track
artists, the live bootleg filed under
its filename. Nine times out of ten,
"Navidrome organised my music wrong"
means "my tags say three different
things and Navidrome believed all of
them."

This is a defensible design, not a
quirk. Folders are one person's filing
habits; tags travel with the file, mean
the same thing on every player, and
were the collection's real catalogue
long before Navidrome arrived. The
server just refuses to pretend
otherwise.

## The tags that matter

Navidrome reads the standard fields.
In rough order of damage when they're
wrong or missing:

| Tag | What it decides |
|---|---|
| **Album artist** | Which artist page an album lives under. The single most important tag for a tidy library — compilations and guest appearances behave because of it. |
| **Artist** | Who performed this track. For most albums, the same as album artist; where they differ, the album still files under album artist. |
| **Album** | The album's name. One stray space or a "(Deluxe)" on some tracks only will split an album in two. |
| **Title** | The track's name. Missing, the filename stands in (as `mystery-tone` demonstrated). |
| **Track number** (and disc number) | Playing order. Missing numbers mean alphabetical order, which is wrong in a way you will hear. |
| **Date / year** | The year shown beside the album, and the order albums line up in on an artist's page. |
| **Genre** | The genre shelves. As messy in most collections as in most record shops; tidy it only if you'll browse by it. |
| **Cover art** (embedded, or a `cover.jpg` beside the files) | What the player shows. Chapter 10 notes the order Navidrome looks in. |

Two habits prevent most library messes.
Keep the album artist **identical**
across an album — including its
punctuation. And on compilations, set
album artist to one name (the
convention is "Various Artists") while
each track keeps its own artist; that
single move is the difference between
one album and twenty one-track ghosts.

## A folder layout that serves two masters

The server is indifferent to folders,
but you are not: you will browse these
folders with human eyes when you add,
fix, and back up music. The layout the
specimens used, and the one this guide
recommends, is the dull classic:

```
music/
  Artist Name/
    Album Name (Year)/
      01 - First Track.flac
      02 - Second Track.flac
      cover.jpg
```

Dull is the point. It makes a missing
file findable, a backup checkable, and
the `.m3u` playlists of chapter 8 easy
to write, since their relative paths
read naturally. It also matches what
most ripping and tagging tools produce
by default, so keeping it costs no
effort. But hear the chapter's refrain
once more: this layout is for *you*.
Rename every folder tomorrow and the
library on screen will not change by
one pixel, because the library was
never reading the folders.

## Fixing tags

Navidrome won't do it — chapter 1 put
tag editing outside its job — so the
work happens in the files, with tools
built for it, and then a rescan reads
the corrections. The respectable
options:

- **MusicBrainz Picard** (Windows, Mac,
  Linux; free) — the librarian's choice.
  It identifies tracks against the
  MusicBrainz database and writes
  consistent tags in bulk. Its defaults
  produce exactly the album-artist
  discipline described above.
- **beets** (command line; free) — the
  librarian for people who like
  terminals. Imports a collection,
  identifies it against MusicBrainz,
  fixes tags, and can rename files into
  the layout above as it goes.
- **Mp3tag** (Windows, and a Mac
  version; free/inexpensive) — the
  scalpel: select fifty files, fix one
  field, done. Best for surgical repairs
  on an otherwise healthy library.
- Your file manager and a rainy hour,
  for a collection small enough that the
  first three are overkill.

(Documentary honesty: this lab's
fixtures were tagged by `ffmpeg` as
they were generated, and none of the
tools above were run on the bench. They
are named from their reputations and
the project's own documentation, not
from a lab trial.)

## Scanning, rescans, and the watcher

Day to day, you will rarely ask for a
scan: the **watcher** notices new and
changed files within seconds — the
lab's dropped-in playlist arrived in
about a second (t12), and new tracks
fare the same. Three scan facts worth
keeping:

- A **quick scan** (the default, and
  what the watcher triggers) reads
  changed files. A **full scan**, which
  the web interface's scan button and
  the `navidrome scan` command offer,
  re-reads everything — the right move
  after bulk tag surgery with an outside
  tool, so no edit is missed because a
  timestamp looked unchanged.
- Tag fixes apply the same way: edit
  the files, rescan, and the library
  re-files itself. Nothing inside
  Navidrome needs rebuilding by hand.
- Removing a file doesn't instantly
  delete its catalogue entry; the
  scanner marks it **missing**, and a
  later cleanup (the `navidrome missing`
  command exists for exactly this) takes
  the entries away. That pause is a
  kindness when a drive is merely
  unplugged for an afternoon.

**Check:** pick an album in your
library, open one of its files in any
tag viewer (or `ffprobe file.flac`, as
the lab did), and compare. The artist,
album artist, and track number on the
file should predict the library exactly.
When they do — and they will — you hold
the whole model in your hand.
