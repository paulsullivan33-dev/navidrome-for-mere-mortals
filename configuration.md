# Configuration

**Scope:** the knobs that actually get
turned — where settings live, the
privacy switch, the defaults worth
knowing before they surprise you, and a
standing warning against collecting
options you don't need.

## Where settings live

Three places, one winner. From the
project's documentation, settings are
taken in this order of precedence:

1. **Environment variables** — the
   config name, uppercased, with `ND_`
   in front: `ND_MUSICFOLDER`,
   `ND_PORT`, `ND_LOGLEVEL=debug`.
2. **Command-line flags** — the same
   names, lowercased: `--musicfolder`,
   `--port`.
3. **The config file** —
   `navidrome.toml`, read from the
   folder you start in (or wherever
   `-c` points).

The file is the right home for a
standing install; environment variables
are how Docker installs are usually fed
(chapter 3); flags are for experiments.
Whichever you use, `navidrome --help`
prints the full list with defaults, and
the project's configuration page is the
authority this chapter samples from —
it is not a copy of it.

## The privacy switch

The one option this guide actively
recommends deciding, rather than
drifting into:

```toml
EnableExternalServices = false
```

By default it is **true**, and "external
services" is a bundle: the metadata
agents that fetch artist biographies and
pictures (Deezer, Last.fm, ListenBrainz,
in that default order), the login
page's background images, and the
project's **anonymous usage statistics**
(`EnableInsightsCollector`, separately
switchable, also on by default). The
lab watched the bundle work: its log
asked Deezer about three invented
artists within seconds of the first
scan (transcript t10). Nothing about
your *files* ever leaves the machine —
the queries carry artist names — but
"the server tells a stranger whose
music I own" is a sentence some
households will want to be false.
Setting the switch to `false` costs
you the biographies, the pictures, and
the pretty login screen; artist images
can still come from files in your own
library. Scrobbling to Last.fm or
ListenBrainz, if you set it up later
from the web page, is a separate choice
made per user, and is documented, not
run, in this guide.

## Defaults worth knowing

Measured at the bench or read from the
project's table, the ones that explain
later behaviour:

| Setting | Default | Why you'll care |
|---|---|---|
| `Port` | 4533 | The address every chapter assumes. |
| `MusicFolder` | `./music` | A wrong path doesn't stop the server — it serves an empty library and complains in the log (t10). |
| `AutoImportPlaylists` | true | Why `.m3u` files become playlists (chapter 8). |
| `DefaultDownsamplingFormat` | `opus` | Why a request for 48 kbps produced Opus, not MP3 (t05). |
| `TranscodingCacheSize`, `ImageCacheSize` | 100 MB each | The cache caps from chapter 2. Generous for most homes; huge cover-art collections may want more image cache. |
| Login rate limit | 5 tries per 20 seconds per address | The boot log announces it. It is why password guessing is a poor hobby, and why a mistyped app password locks *itself* out briefly rather than you forever. |
| `Backup.Schedule`, `Backup.Count`, `Backup.Path` | off | Automatic database backups exist but are **not on** until you set them — chapter 11 builds the habit deliberately instead of discovering this row afterwards. |

## A word about the rest of the list

The full table runs to dozens of
options — themes and UI defaults,
ReplayGain display, artwork priorities,
share expirations (chapter 8's year is
`DefaultShareExpiration`), headers for
reverse-proxy authentication, an
experimental second API borrowed from
Jellyfin's clients, and more. Almost
all of it is for a server that has
outgrown chapter 3's three lines, or
for tastes this guide shouldn't pick
for you. The discipline that keeps a
home server pleasant: add a setting
when you can name the behaviour it
changes and the day you'll check it
did. A config file collected like
stamps rots like milk.

**Check:** set `LogLevel` to `debug`,
restart, and watch one scan and one
stream go by; then set it back. You
should feel the difference between
*operating* the server and *guessing
at* it — that feeling is the whole
point of the log, and chapter 16 leans
on it.
