# Users and Permissions

**Scope:** the second account, the exact
shape of "not an admin" — which doors it
closes and which it provably leaves
open — measured against the running
server, not inferred from a manual.

## The model, in one paragraph

Navidrome has exactly two kinds of user:
**admins**, who can do everything,
including making more users; and
**listeners**, who can browse and play
the shared library, keep their own
playlists, stars, ratings, and play
counts — and that's the list. There are
no per-artist or per-album permissions,
no private libraries inside one server,
and no read-only mode finer than the
fences below. This chapter exists
because those sentences are easy to
believe wrong in both directions, so
the lab measured them.

## Kenji, the second user

Transcript t07 created the lab's second
account the way you will: signed in as
the admin, through the same native API
the web page's user screen uses —
`kenji`, name and email attached,
`isAdmin: false`. He signed in on the
first try and the server reported him,
correctly, as not an admin. Everything
after that in this chapter is his
session, poking at fences.

## The fences, tested

**He cannot see or touch the users.**
Asking for the user list with his token
returned `403 — "Error reading user:
Permission denied"`; trying to *create*
a user (the lab cheekily attempted a
third account, flagged admin) returned
`403 — "Saving user: Permission
denied"` (transcript t08). The admin
corner of the web page doesn't even
render for him. This is the fence that
matters most in a shared house: a
borrowed login cannot mint itself
reinforcements.

**He cannot see Ruth's private
playlist.** His playlist list, asked
through both APIs, was empty. And when
the lab asked for Ruth's playlist
*by its exact ID* — no browsing, the
address itself — the server answered
`playlist not found` (Subsonic error
70): not "that's not yours," but a flat
denial that it exists. Private means
private in the strong sense: unlisted,
unguessable in conversation, and
denied by ID.

**What he makes is his own.** Kenji's
stars, ratings, playlists, and play
counts begin empty and accrue to him
alone (his empty starred list is
transcript t09). Deleting a listener
deletes that drawer of theirs;
the library, and everyone else's
drawers, are untouched.

## The open field, also tested

And now the direction people get wrong:
**Kenji can play everything.** The
library is a commons. His search found
the same tracks (t08), his stream of a
song Ruth had scanned returned the full
file, and his phone app would show the
identical artists and albums as hers.
There is no supported way in one
Navidrome to say "this artist is not
for the children" or "this folder is
mine alone." If a household needs
separate libraries with separate
contents, the honest architecture is
two Navidrome servers (the program is
small; chapter 2) — or one server and a
social agreement. This guide's job is
to say which of those you're buying
*before* the argument, not after.

## Running a household server, briefly

- **One admin, held by the person who
  maintains the machine.** Everyone
  else — including, on the advice of
  chapter 4, you for daily listening —
  gets a listener account.
- **Passwords are per person, from the
  start.** The Subsonic token is derived
  from the password (chapter 7), so a
  changed password evicts every app that
  knew the old one: the lost-phone
  drill is "change Kenji's password,"
  not "hunt for a device list."
- **The admin can reset, the CLI can
  rescue.** An admin can edit any user
  in the web page; if the admin password
  itself is lost, the binary's
  `navidrome user edit` is the way back
  in — the user commands work directly
  on the database, and the lab's
  `navidrome user list` ran happily
  against the live server (transcript
  t14).
- **Removal is clean.** Deleting a user
  in the admin screens removes their
  account and their per-person data.
  Their imported `.m3u` files, if any,
  are just files — chapter 8's rules,
  not the account's.

**Check:** sign in as your listener
account and try to open the admin's
user-management screen — it shouldn't
exist for you. Then play anything at
all. Both results are the system working
as this chapter described: a narrow
admin door, a wide listening field.
