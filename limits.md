# Limits

**Scope:** what Navidrome will not do,
will not survive, or will not protect
you from — stated before you build a
household routine on top of it.

## The library is shared, full stop

There are no private collections, no
per-user folders, no parental controls
inside one server. Chapter 9 measured
it: any listener can stream any song.
If that shape doesn't fit your house,
the answer is a second server
(chapter 13, recipe 4), and no amount
of configuration will talk one server
into being two. Designs that pretend
otherwise — one library, "hidden"
folders — do not exist here, and
third-party advice implying they do
should be distrusted on sight.

## It serves; it does not shelter

Navidrome is a streaming server, not an
archive. It will never re-rip your CDs,
never repair a rotting file (chapter
16's `broken.mp3` streamed its text
bytes back with a straight face and an
HTTP 200), never transcode your
collection to safer formats as a
standing job, and never back anything
up on its own until you set the timers
in chapter 11. Bit-rot in the music
folder is your backup routine's
problem, exactly as it was before
Navidrome arrived.

## The exposure limit

A login page is not a fortress, and
this guide's own bench is the evidence
for the defence's strength and its
ceiling: real accounts, hashed-token
app sign-in, a login rate limit of five
tries per twenty seconds (chapter 10).
That is a reasonable lock on a house
door *inside a network you control*.
Forwarding port 4533 from the public
internet puts that lock on a street
with professional burglars:
credential stuffing, scanner bots, and
tomorrow's unpatched vulnerability
all get a turn. Navidrome has no
two-factor authentication and no
security team. The grown-up ways to
listen away from home are a VPN back
to the house, or a reverse proxy with
its own authentication in front —
pointed at, not taught, in chapter 6 —
or the offline downloads of chapter
13, which need no door at all. Choose
one of those; the fourth option is
hope.

## The outside-world dials are all-or-nothing-ish

Chapter 10's `EnableExternalServices`
is a bundle, not a tasting menu:
biographies, pictures, login art, and
usage statistics travel together, and
switching it off takes all of them. You
can trim around the edges (the insights
collector has its own switch) but the
coarse grain is a fact of the current
version, and the guide notes it as a
limit rather than discovering it as a
betrayal.

## One process, one point of failure

One binary is the blessing (chapters 2
and 12) and the limit: when Navidrome
stops, listening stops — for every
user, every app, at once. There is no
cluster mode, no failover, no read
replica. For a household this is the
correct trade; for anything that
resembles a service other people
depend on, it is the ceiling. The
mitigations are chapter 11's backups
and chapter 12's rehearsed swap, which
make the failure a ten-minute story
instead of a lost library — but a
story it remains.

## Format and metadata edges

The scanner reads the common formats
and the standard tags; exotic formats,
unusual tag frames, and truly antique
files can scan imperfectly or not at
all, and some formats' cover art and
lyrics behaviour differs (the
project's documentation keeps the
current list). ReplayGain, gapless
playback, and similar refinements
depend on tags and on the client app
as much as on the server. The lab's
seven tame specimens proved the
mainstream paths; your 2007-vintage
oddity is unproven territory, and
chapter 16's discipline — read the
file with `ffprobe`, read the log,
believe both — is the map for it.

## Scale, honestly bounded

Navidrome is known to serve very large
collections, and the catalogue is just
SQLite rows; but this guide's bench
held seven songs, and it will not
invent numbers for a hundred thousand.
What the design promises is also what
chapter 2 showed: scanning cost rides
on file count and tag health, serving
cost rides on simultaneous listeners
and transcoding, and both are modest
per unit. If your collection is
enormous, the first full scan taking
its time is normal, and chapter 10's
log is how you watch it prove itself.

**Check:** read the six headings again
and ask which one, if it bit, would
actually ruin your week. For most
houses the answer is the exposure
bullet — which is why this guide put
a whole chapter (11) behind the backup
routine and a fence (9) inside the
house, and left the street door to a
VPN.
