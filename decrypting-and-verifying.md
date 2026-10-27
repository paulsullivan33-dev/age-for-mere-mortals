# Decrypting and Verifying

By the end of this chapter you will have
a method — not a hope — for knowing a
locked file opens and is whole, and you
will be able to read age's failure
messages and say exactly which of four
things went wrong.

## Decryption, the whole surface

```bash
age -d -i my-key.txt -o note.txt note.txt.age
```

Three habits do most of the work:

- **`-i` can be repeated.** Carry
  several identities — your own, the
  team's spare, an SSH key — and age
  tries each against the stanzas until
  one unwraps. There is no penalty for
  offering the wrong ones first; they
  simply do not match.
- **The identity can be `-`.** age
  reads an identity from standard input
  (`-i -`), which is how scripts hand
  it a key from a vault or a prompt
  without writing it to disk (chapter
  11).
- **No `-o` means standard output.**
  Decryption can stream straight into
  the next program, which is both a
  scripting tool and the verification
  method below.

## Look before you open: age-inspect

`age-inspect` reads a locked file's
outside — no identity needed, nothing
decrypted:

```text
note.txt.age is an age file, version "age-encryption.org/v1".

This file is encrypted to the following recipient types:
  - "X25519"
...
    Header                       168 bytes
    Encryption overhead           32 bytes
    Payload                       53 bytes
```

(Transcript t04, including the armored
file's extra "Armor overhead" line.)
Reach for it when a file arrives
mysteriously: it tells you whether the
file is an age file at all, how many
and what kind of stanzas it carries,
whether it is armored, and whether it
uses post-quantum recipients. It will
never tell you who the recipients are
or what is inside — that part is not
yours to read without a key.

## The verification method

`diff` proved the toy examples. For
anything real, compare **hashes** — a
fingerprint of the contents that any
tool can recompute. The method, run by
the lab in transcript t11:

```bash
sha256sum birthday-video.mp4
age -d -i my-key.txt birthday-video.mp4.age | sha256sum
```

Two identical hashes, and the streamed
decryption is bit-for-bit the file you
locked — without ever writing the
plaintext to disk a second time. Do
this on the day you make an important
archive, and again whenever you restore
from it. It takes a minute and it is
the difference between a backup and a
story about a backup.

## The four failures, decoded

age's error messages are short, and
each names a different disease:

- **`no identity matched any of the
  recipients`** — the identities you
  offered do not correspond to any
  stanza in this file. Wrong key file,
  or the file was never locked to you.
  The lab produced this on purpose with
  a stranger's key (transcript t09):
  exit code 1, and no output file
  written.
- **`incorrect passphrase`** — the file
  is passphrase-locked and the phrase
  was wrong (transcript t05). Same
  discipline: exit 1, nothing written.
- **`failed to decrypt and authenticate
  payload`** (wording varies slightly by
  version) — a stanza unwrapped, but the
  contents did not pass their seal: the
  file is damaged or was tampered with
  after locking. This is the cipher
  doing chapter 2's promise. Treat the
  copy as lost and go to another one.
- **Not an age file at all** — inspect
  says so, or decryption fails at the
  header: someone renamed a file, or a
  transfer mangled it, or it was armor
  with its first or last line clipped
  by a chat program (chapter 16 has the
  repair habits).

One more, the quiet success that looks
like a failure: decryption "hangs" at a
passphrase prompt you did not expect —
the file is passphrase-locked, or the
identity is an SSH key with a passphrase
of its own (chapter 9). age is waiting
for typing, not broken. Chapter 16
collects these cases.
