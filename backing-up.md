# Backing Up

**Scope:** the three things there are
to lose, in ascending order of tears —
the config file, the database, the
music — the backup command the binary
carries, and a restore that this lab
actually rehearsed, including the
mistake it made first.

## What there is to lose

Chapter 2's folders, restated as a
backup policy:

| Thing | Where | Replaceable? |
|---|---|---|
| The config file | `navidrome.toml` (or your env vars) | Yes — a dozen lines, retyped in minutes. Back it up anyway; minutes are still minutes. |
| The database | `navidrome.db` in the data folder | **Only from a backup.** Users, passwords, playlists, stars, ratings, play counts, and the whole catalogue live here and nowhere else. |
| The music | the music folder | Only by re-ripping or re-buying it. Navidrome's backups never include it — it is *your* archive to protect, with ordinary file backup. |

The cache is deliberately not on the
list. Whatever backup routine you adopt
for the data folder can skip
`data/cache` entirely.

One technicality with teeth: while the
server runs, the database keeps working
papers beside it (`navidrome.db-wal`,
`-shm`). Copying the three files of a
*live* database by hand can produce a
backup that is quietly inconsistent.
The two clean ways are the ones below:
the server's own backup command, or
stopping the server for the copy.

## The built-in backup

The binary carries the job:

```console
$ navidrome backup create -c navidrome.toml --backup-dir ./backups
… msg="Backup complete" … path=…/backups/navidrome_backup_2026.10.22_23.42.19.db
```

(The lab's transcript t11 is the real
run: one file, about three-quarters of
a megabyte for its specimen library,
written in well under a second.) The
companion commands prune and restore:

```console
$ navidrome backup list
$ navidrome backup prune
$ navidrome backup restore -c navidrome.toml --backup-file <file> --force
```

And for hands that prefer a calendar to
a memory, the configuration offers
`Backup.Schedule` (a cron spelling),
`Backup.Count` (how many to keep), and
`Backup.Path` — automatic backups, off
until set (chapter 10). Either way the
backup is only the database: combine it
with your ordinary file backup of the
music folder and a copy of the config,
and the set is complete.

## The restore, rehearsed

This guide does not trust an unrehearsed
backup, so the lab restored into a
fresh folder and a second port and
asked the restored server who it was.
The full record is transcript t11; the
shape of it:

1. The first attempt failed honestly,
   and the failure is the lesson:
   `backup restore` against a data
   folder with **no database in it**
   stops with `No existing database`.
   Restore *replaces* a database; it
   does not conjure one into an empty
   folder. (The reasons are the tool's
   own; the fix is the next step.)
2. Start the new instance once, so it
   creates its own fresh database,
   then stop it. *Now* restore, with
   `--force` to confirm the replacement.
   `Restore complete`.
3. Start it again and inspect the
   inheritance: Ruth's password signed
   in. Kenji's password signed in. The
   playlist **Evening Specimens** was
   there, with the same ID it had on
   the old server. Ruth's star on
   "High Ping" was still starred. The
   library searched complete.

Everything a listener can lose came
back, because everything a listener has
*lives* in that one file. The restore
took under a second of tool time; the
rehearsal is the expensive part, and it
costs one quiet afternoon per major
change of routine, which is the correct
price.

Two footnotes the rehearsal earned.
The restore must be done **offline** —
the command says so itself — meaning
the target server stopped while its
database is replaced. And a backup
file is a SQLite database in a coat:
copying it into a fresh data folder as
`navidrome.db` by hand achieves the
same end, which is worth knowing on the
day the tidy way is unavailable.

## The standing routine

What this guide recommends, in the
series' usual Sunday shape:

- `Backup.Schedule` on, or a weekly
  cron/systemd timer running
  `backup create`, keeping a handful of
  generations (`Backup.Count`, or your
  own pruning).
- The backups folder inside your
  ordinary file backup, so it leaves
  the machine. A backup on the dying
  disk is a souvenir.
- The music folder inside the same
  ordinary backup, checked the way
  chapter 5's layout makes easy.
- Once — once is enough to learn the
  steps — a restore rehearsal like the
  lab's, on a spare port, after which
  you may delete the restored copy and
  the worry together.

**Check:** your backup is real when you
can answer, without looking: where is
last Sunday's database backup, and
have you ever seen one restore? If the
second answer is no, the rehearsal
above is an hour well spent before the
library grows teeth.
