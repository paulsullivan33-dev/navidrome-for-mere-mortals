# Listening with Apps

**Scope:** the second door into the same
server — the **Subsonic API** — how apps
sign in, what streaming really moves
over the wire, transcoding, and a short
honest list of client apps this lab did
not test.

## One server, two doors

Chapter 6's web page is the front door.
This chapter is the side door, and for
daily listening it is usually the better
one: the **Subsonic API**, a protocol
older than Navidrome itself. Subsonic
was a music server of the 2000s; its API
outlived it and became a lingua franca.
Any app that speaks it — and there are
many, on every platform — can treat your
Navidrome as its library. The server
itself answers to both doors from the
same binary: the boot log mounts them
side by side, "Native API routes" at
`/api` and "Subsonic API routes" at
`/rest`.

The web page, pleasingly, is itself a
Subsonic citizen: at sign-in it stores a
per-user Subsonic token, and its player
streams through the same machinery the
apps use. Everything proved in this
chapter was proved once, for both.

## How an app signs in

An app asks for three things: the
server's address (`http://<your-server>:
4533`), a username, and a password. It
never sends the password in the clear
after setup. Instead, for each request,
it invents a random **salt**, hashes
password + salt into a **token**
(`md5(password + salt)`, in the
protocol's traditional spelling), and
sends username, token, and salt. The
server, which knows the password,
checks the hash. Transcript t03 is the
whole handshake done by hand with
`curl`:

- `ping` with the correct token →
  `status: "ok"`, and the server
  announcing itself: type `navidrome`,
  version `0.64.2`, `openSubsonic: true`.
- `getUser` → the account's shape in the
  protocol's old vocabulary: `adminRole`,
  `streamRole`, `downloadRole`,
  `shareRole`, and friends. App features
  grey themselves out according to these
  flags, which is why one app shows a
  download button and another doesn't.
- The same call with a wrong password →
  error 40, from chapter 4.

Practical consequence worth knowing:
because the token is a hash of the
password, changing your password
invalidates every app's stored sign-in
at once — a feature, when a phone is
lost, worth the minute of re-typing on
the devices you kept.

## Streaming, byte for byte

Pressing play in an app calls
`/rest/stream` with a song's ID. For an
unconverted stream, Navidrome serves
the file itself — not a re-encode, not
a "quality tier," the file. The lab
checked the strongest version of that
claim: the streamed copy of the FLAC
specimen has the **same SHA-256
checksum** as the file on disk,
byte for byte (transcript t04). What
your DAC receives from a FLAC library
is the FLAC.

## Transcoding: the graceful downgrade

Sometimes the original is the wrong
thing to send — a FLAC over mobile
data, a format the app can't decode.
Subsonic's answer is `maxBitRate`: the
app names a ceiling, and if the file
exceeds it, the server converts on the
fly with ffmpeg (chapter 2's one
dependency). The lab asked the FLAC for
48 kbps and received **Opus at 48
kbps** — Navidrome's default
downsampling format in this version —
while the server's log wrote
`transcoding=true … format=opus …
bitRate=48` (transcript t05).
Converted streams are cached (chapter
2's transcoding cache), so the second
listener at the same setting doesn't
make the server re-convert.

Rules of thumb, stated without
benchmarks the lab didn't run: on any
machine from the last decade, a few
simultaneous conversions are nothing;
a roomful of listeners all transcoding
is where an old spare-parts box starts
to feel it. Apps usually expose the
setting as "Wi-Fi quality" and "mobile
quality" — set mobile lower and let
the server earn its keep.

## The clients (documented, not tested)

This lab exercised the protocol
directly and installed **none** of the
apps; the list below is the project's
ecosystem as documented, offered as a
map, not a verdict. Names change and
apps retire; the protocol is the
durable part.

| Platform | Known Subsonic clients |
|---|---|
| Android | Symfonium, DSub, Subtracks, Tempo |
| iPhone / iPad | play:Sub, Substream, Narra |
| Desktop (Win/Mac/Linux) | Supersonic, Sublime Music, Strawberry (as a source) |
| In the car / TV | Android Auto and CarPlay via the phone apps above; various TV players |

Two questions choose among them better
than any review: does it **download for
offline play** (most cache or pin
albums — the difference between a
library that flies with you and one
that stops at the door), and does it
handle **transcoding settings** the way
you travel. Point any of them at your
server's address with your ordinary
(non-admin) account from chapter 9.

**Check:** with any client connected,
find the same song two ways — once in
the browser, once in the app — and play
both. Same server, same file, two
doors. If the app can't connect at all,
the suspects in order are the address
(LAN address vs. away-from-home access),
the password, and whether the server is
even up — chapter 16 takes it from
there.
