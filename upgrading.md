# Upgrading

**Scope:** the whole upgrade — because
there is barely one. Stop the program,
swap the file, start the program, and
watch the database migrate itself.
This lab rehearsed it for real, from
the previous release to this guide's.

## The rehearsal

Transcript t13 is the complete event,
and it is short because the event is
short. The lab:

1. Started **v0.63.2** (the previous
   release line) against a fresh data
   folder, and made an admin the usual
   way.
2. Stopped it.
3. Started the **v0.64.2** binary
   against the *same* folder, on the
   same port.
4. Read the log: `goose: successfully
   migrated database to version
   20260922230428` — the migration
   tool the project uses, reporting that
   the old database had been carried
   forward to the new schema, in the
   boot second.
5. Checked the inheritance: the admin
   password worked unchanged; a Subsonic
   ping reported the server as
   `0.64.2`; the library answered
   searches.

No export, no import, no migration
script run by hand, nothing to re-enter.
The database's schema version rides
inside the database; a newer binary
sees an older schema and lifts it
before serving a single request.

## The standing procedure

For a real install, the rehearsal plus
chapter 11's routine, in order:

1. **Take a backup first**
   (`navidrome backup create`, or
   confirm last night's automatic one is
   there). Upgrades are where the
   database changes shape; the backup is
   the only way back across that change.
2. **Stop the server.** On systemd,
   `sudo systemctl stop navidrome`; in
   the lab, a signal to the process.
   Upgrades are not a restart-while-
   running affair — the old and new
   binaries must never hold the folder
   at once.
3. **Swap the binary** for the new
   release's, checksum-checked as in
   chapter 3. Keep the old binary beside
   it under its versioned name for a
   day; disk is cheap, regret is not.
4. **Start it and read the first
   minute of log.** You are looking for
   the migration line and the "server is
   ready" line, in that order, with
   nothing red between them.
5. **Sign in and spot-check** one
   playlist, one star, one streamed song.
   The rehearsal's three questions,
   answered in under a minute.

Docker installs (documented route,
chapter 3) follow the same shape with
different hands: pull the new image,
recreate the container with the same
volumes, read the same log. The data
volume is the data folder; the program
is the program.

## Going backwards

Shorter than the way up, and worth
knowing before you need it: a database
that has been migrated forward
generally cannot be read by the older
binary — schema migrations are
one-way doors. Downgrading is therefore
not "run the old binary," it is
**restore the pre-upgrade backup into
the data folder, then run the old
binary** (chapter 11's restore, offline,
with `--force`). This is the entire
reason step 1 exists, and the reason
this guide's upgrade procedure is
allowed to be four lines long: the
exit is rehearsed, not hoped for.

Release notes deserve their minute too:
the project publishes them with each
release, and a two-minute read for
"breaking" or "migration" before an
upgrade is the difference between a
routine swap and a surprise. The
project's upgrade page also carries
version-specific warnings when a
release needs one (a historical
example: very old 0.4x databases were
advised to pass through intermediate
versions) — the long-jump case is the
one to check when a server has been
left alone for years.

**Check:** after an upgrade, the ping
should report the new version (the lab
read `0.64.2 (10114574)`), your oldest
playlist should be intact, and the log
should show exactly one migration. If
any of the three is missing, stop and
reach for the backup *before* poking
further — a half-migrated database is
the one state this chapter's calm
doesn't cover, and chapter 16 is the
next stop.
