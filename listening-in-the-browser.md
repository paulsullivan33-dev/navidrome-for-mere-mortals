# Listening in the Browser

**Scope:** the web interface — what it's
for, what you'll find on it, and an
honest note on how this guide knows:
the lab drove the interface's own API
rather than clicking its buttons, and
the chapter says so where it matters.

## Why the browser matters

Every Navidrome has the same first
listening post: the **web interface** at
`http://<your-server>:4533` (it forwards
to `/app/`). It needs no install, works
on any machine on your network, and is
the place where administration happens —
users, settings, scans. Even if your
daily listening happens in an app
(chapter 7), the browser is where the
server is *run from*. Learn it first.

## What you'll find there

The interface is a small streaming
service in the familiar shape, and this
guide will not narrate every button —
it is genuinely easy to wander. The
landmarks:

- **Home** — the recent and the loved:
  recently added albums, recently
  played, most played, random albums.
  On a new server it's a portrait of
  your first scan.
- **Artists, Albums, Songs** — the
  library browsed three ways, all of it
  built from tags (chapter 5). Artist
  pages carry the biography and picture
  the outside agents fetch, when they're
  allowed to (chapters 4 and 10).
- **Playlists** — yours, and the ones
  chapter 8 is about, including any
  `.m3u` files the scanner imported from
  the music folder.
- **Search** — one box over everything.
  It queries the same catalogue the
  lab's Subsonic searches hit in
  transcript t03, and finds the same
  things.
- **The player** — a bar along the
  bottom: play, skip, a queue you can
  rearrange, a star for favourites, and
  a volume. Playback streams from your
  server at the file's own quality
  unless something asks it to convert —
  in the browser, nothing does by
  default, so FLAC arrives as FLAC (the
  lab's byte-for-byte proof is
  transcript t04).
- **The admin corner** — under your
  name: user management, server
  settings, the manual scan button, and
  monitoring (including who is listening
  right now, if the now-playing feature
  is on). Chapter 9's fences were built
  here: an ordinary user simply never
  sees this corner.

Sign in is the account from chapter 4
(or your own ordinary account, if you
took its advice). Stay signed in and the
browser remembers you; the session token
it holds is the same kind the lab used
for every native API call.

## How this guide knows (and its limit)

Stated once, plainly, because
`criteria.md` insists: **the lab did not
click through this interface.** A
sandbox has no friendly browser for
screenshots, and a guide that faked
them would be lying in pictures. What
the lab did instead is arguably
stronger for the factual claims: every
screen above is a window on the native
API, and the lab exercised that API
directly — login, library, playlists,
user administration, and their error
messages (transcripts t01, t02, t06–
t08). The interface itself was fetched
and answered, identifying its version in
its own page furniture (v0.64.2). The
descriptions of layout follow the
project's documentation. If a button
has moved a few pixels in your version,
the map is still right even where the
furniture has shifted.

## Two browser habits worth forming

- **Check the library here first.**
  When an app shows something odd, the
  browser is the referee: if it's wrong
  here too, it's the tags or the server
  (chapters 5 and 16); if it's right
  here, the app is the problem.
- **Remember it's an open door on your
  network.** Anyone who can reach the
  address gets a login page. That's
  fine at home and chapter 15 is blunt
  about why the raw port should not be
  forwarded to the internet: Navidrome
  has logins and rate limits, not a
  security team. The usual answer is a
  VPN or a properly configured reverse
  proxy — both subjects of their own,
  pointed to and not taught here.

**Check:** sign in, open an album, and
play it. The player should show the
right cover, the right track order, and
the artist the tags promised. Then star
a song — you just used the machinery
chapter 8 takes apart.
