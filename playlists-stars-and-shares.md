# Playlists, Stars, and Shares

**Scope:** the three things you make
from a library — playlists (including
the ones that arrive as files), stars
and ratings (which are per-person, and
provably so), and public shares, which
this lab did **not** exercise and which
are labelled accordingly.

## Playlists, two ways in

A playlist in Navidrome is an ordered
list of song IDs and a name — nothing
more exotic — and they arrive by two
routes:

**Made inside.** In the web page or an
app, gather songs and save the list.
Underneath, that is one API call; the
lab made its specimen — "Evening
Specimens," both FLAC tones and the
small MP3 — with the Subsonic
`createPlaylist` call and then read the
*same* playlist back through the native
API the web page uses (transcript t06).
One list, both doors, like everything
else in this guide. A playlist made
this way is born **private**
(`"public": false` in its record):
chapter 9 shows what that privacy
provably means.

**Imported from a file.** Drop an
`.m3u` file into the music folder and
the scanner adopts it. The lab's
`imported.m3u` — three lines of relative
paths under an `#EXTM3U` header — was
picked up by the watcher within
seconds and appeared as a playlist
named `imported`, owned by the admin,
marked **synced** and **read-only**
(transcript t12). The spellings matter:

- *Synced* means the file stays the
  source of truth: edit the `.m3u` and
  the playlist follows on the next scan;
  delete the file and the playlist goes
  with it.
- *Read-only* means the web page and
  apps won't let you edit its contents
  — there is nothing to edit *into*;
  the editor is a text editor, on the
  file.
- The paths inside are read relative to
  the music folder, in the
  `Artist/Album/track` shape of chapter
  5. Lines that match nothing in the
  library are silently skipped, which
  makes a typo look exactly like a
  missing song until you count.

Auto-import is on by default
(`AutoImportPlaylists`, chapter 10 can
turn it off), and chapter 4 explained
its one delay: before the first admin
exists, imports wait, because a playlist
must belong to someone.

## Stars, hearts, and ratings

The small decorations: a **star** (apps
say "starred," the web page draws a
heart — same machinery; Subsonic's
vocabulary is simply older) marks a
song, album, or artist as a favourite,
and **ratings** give 1–5 stars in the
other sense. Both are *yours*: stored
against your account, invisible to
other listeners, and the backbone of
the "favourites" and "top rated"
shelves.

Per-person is a claim, so the lab
checked it: Ruth starred "High Ping,"
and Kenji's starred list — asked for in
the same minute, on the same server —
came back empty (transcript t09). His
stars will be his own when he makes
them. Play counts and "recently played"
live in the same per-person drawer.

## Shares (documented, not run)

Navidrome can mint a **public link** to
a song, album, or playlist: anyone
holding the link can stream it in a
bare web player without an account.
The routes exist (the boot log mounts
`/share`), the feature is on by default
(`EnableSharing`), links default to a
year's life, and a user's `shareRole`
flag — visible in chapter 7's `getUser`
reply — decides whether that user may
create them at all.

This lab did not create one, and the
guide won't pretend otherwise: a share
link is a door deliberately left ajar
in a server chapter 6 already warned
about exposing, and minting one on a
bench proves little beyond the click.
If you use shares, the habits that keep
them sane are: shorten the expiry
(`DefaultShareExpiration`, chapter 10),
take the share role away from users who
don't need it, and remember that a link
forwarded is a link escaped — it is a
capability, not a message to a person.
Managing and revoking shares lives in
the web interface's sharing screens,
described in the project's own docs.

**Check:** make a playlist in the
browser, then open your phone app: it
should be there, in the same order,
before you've finished reading this
sentence. Then star a song in the app
and look for the heart in the browser.
Two doors, one house — if either half
of that fails, chapter 16.
