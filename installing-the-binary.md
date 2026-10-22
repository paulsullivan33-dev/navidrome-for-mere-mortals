# Installing the Binary

**Scope:** the download-and-run install —
the release page, checking the download,
ffmpeg, a three-line config file, the
first start, and making it a standing
service. Docker and the other routes are
named at the end, labelled for what they
are: documented, not run by this lab.

## What you need

Very little, and it is worth saying how
little after the four-container installs
elsewhere in this series:

- A machine that stays on when you want
  to listen — a spare PC, a small home
  server, a Raspberry Pi. Navidrome
  itself is light: the program idles in
  well under a hundred megabytes of
  memory for a household library. The
  music folder wants the disk space, not
  the server. (A claim about *your*
  library's size belongs to your
  collection, not this guide.)
- Your music, in files, somewhere the
  machine can read.
- **ffmpeg**, the one outside tool
  Navidrome depends on, for converting
  streams (chapter 2). Most Linux systems
  install it with `sudo apt install
  ffmpeg` or the equivalent. On the first
  start the log says `Found ffmpeg` —
  transcript t00's boot has it; if yours
  instead warns that ffmpeg is missing,
  that is the first thing to fix.

## The download

The project publishes every release on
its GitHub releases page, as ready-to-run
files for Linux (Intel and ARM, 32- and
64-bit), Windows, and macOS. Pick the
file for your machine — Linux on an
ordinary PC is
`navidrome_0.64.2_linux_amd64.tar.gz`
(this guide's version; yours will be
newer, and that is fine). On an ARM
machine such as a Pi, the project's
installation page advises checking
`cat /proc/cpuinfo` to choose between
the ARM builds.

Check the download before trusting it.
The release also publishes
`navidrome_checksums.txt`:

```console
$ sha256sum navidrome_0.64.2_linux_amd64.tar.gz
fdd87fd107818667c2c50bc24dc6ce856962c795b3822a91f95c066cfad2c53e
```

The lab's file matched the published sum
for v0.64.2 exactly. Yours should match
*your* release's sum in the same way —
if it doesn't, download again and do not
run the file.

Unpack it (any `tar` warning about file
ownership on odd systems is noise; what
matters is the binary):

```console
$ tar -xzf navidrome_0.64.2_linux_amd64.tar.gz
$ ./navidrome --version
0.64.2 (10114574)
```

## The config file

Navidrome reads `navidrome.toml` in the
folder you start it from. Three lines
are a complete, honest start:

```toml
MusicFolder = "/home/you/music"
DataFolder  = "/home/you/navidrome-data"
Port        = 4533
```

`Port` 4533 is the default (the project's
nod to a famous jukebox numbering); leave
it unless something already uses it.
Every setting can also be given as an
environment variable — the config name
with `ND_` in front, uppercased
(`ND_MUSICFOLDER`, `ND_PORT`) — or as a
command-line flag; when they disagree,
environment beats flags beats the file.
Chapter 10 covers the switches worth
knowing. For now, three lines.

Two habits worth starting on day one,
both borrowed from harder lessons in
this series: put the data folder on a
disk you **back up** (chapter 11), and
keep the music folder somewhere it can
grow — moving terabytes later is an
afternoon you don't get back.

## The first start

```console
$ ./navidrome
```

Watch the log settle. A good first boot
ends with:

```
----> Navidrome server is ready!  address="0.0.0.0:4533"
```

and, a moment later, the scanner's first
pass over your music folder
(`Scanner: Finished scanning all
libraries` — on a big collection the
first pass takes its time; let it).
Then open `http://<your-server>:4533` in
a browser. The root address forwards to
`/app/`, the web interface, whose first
screen offers to create the first
account. That is chapter 4.

**Check:** the version command printed
your release, the log reached "server is
ready," and the browser shows a
Navidrome welcome screen rather than a
connection error. If the address refuses
to load, chapter 16 walks the failures —
the three usual suspects are a wrong
path in the config (the server starts
anyway and serves an *empty* library;
the lab proved it, transcript t10), a
port already in use, and a firewall.

## Making it stand

Running a program in a terminal is a
demo, not a service. Two standing
arrangements, either respectable:

- **The built-in route.** The binary
  carries its own service manager
  commands — `navidrome service install`
  registers it with systemd on Linux
  (and the equivalent on other systems),
  so it starts at boot and restarts on
  failure. `examples/navidrome.service`
  in this repo shows the shape of the
  unit it manages, for readers who like
  to see the wiring.
- **Your own unit.** A short systemd
  unit of your own, pointing at the
  binary and config, does the same job
  with nothing hidden. The example file
  works as that too, copied to
  `/etc/systemd/system/` and enabled.

Run it as a **dedicated, unprivileged
user** if the machine is shared at all:
Navidrome only needs to *read* the music
folder and *write* its data folder, and
a music server has no business holding
keys to anything else.

## The other routes (documented, not run)

This lab ran the plain binary, and the
rest of the guide speaks from that.
The project also supports, and its own
documentation covers:

- **Docker.** An official image
  (`deluan/navidrome`) and a compose
  example ship with the project; the
  config file goes in the `/data`
  volume, and the environment-variable
  spellings above are the usual way in.
  `examples/docker-compose.yml` in this
  repo follows the project's published
  example. Not run here — sandbox
  kernels and containers are old enemies
  in this series — but it is the route
  most of the project's users take.
- **Packages.** The release page also
  carries `.deb` and `.rpm` builds of
  the same binary, and community packages
  exist for several NAS systems and
  app stores (TrueNAS, Unraid, Synology
  community sources, and friends).
- **Managed hosting.** The installation
  page lists paid hosts that will run it
  for you, which trades the whole point
  (your machine, your files) for
  convenience; named here only so the
  map is complete.

Whichever route, the program, the three
folders, and everything after this
chapter are the same.
