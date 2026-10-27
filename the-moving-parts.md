# The Moving Parts

By the end of this chapter the inside of
a locked file will hold no mysteries: you
will know what a recipient and an
identity each hold, why one locked file
can open for several people, and what the
header and the armor are.

## Recipients and identities

A key pair is two strings of text.

The **recipient** (the public key) looks
like this, from the lab's own key pair:

```text
age1jf2fmygfh44vq0zkvdfh5jmtaac9n5t8txk28fz6044hak3fe4fsn9agud
```

The **identity** (the secret key) is its
partner, and it lives in a small text
file that `age-keygen` writes:

```text
# created: 2026-10-27T02:51:52Z
# public key: age1jf2fmygfh44vq0zkvdfh5jmtaac9n5t8txk28fz6044hak3fe4fsn9agud
AGE-SECRET-KEY-1A293DP2JKKAGMCU0EK4QCJ4U0M5FV080CFFHN78NWLF935XEKVNQ47880S
```

(Those are the lab's throwaway keys,
printed in transcript t01. Yours are
yours: the recipient is shared freely,
the identity file is a house key —
chapter 1, rule 2.)

Mathematically the pair is built on
**X25519**, a well-studied key-agreement
scheme: the recipient is a point anyone
can lock to, and only the identity can
complete the unlocking side of the sums.
You never do these sums yourself. You
only keep the two strings in their
proper places: recipients travel,
identities stay home.

## The file key: the trick that makes it all work

age does not encrypt your file directly
with the recipient's key. Instead, for
every file, it invents a fresh random
**file key** — 16 bytes that exist only
for this one file. The file's contents
are encrypted with that file key, using
**ChaCha20-Poly1305**, an authenticated
cipher: it scrambles the contents and
seals them so that any tampering is
detected on opening.

Then the file key itself is locked —
**wrapped** — once per recipient, and
each wrapping is written into the file's
header as a **recipient stanza**: a small
labelled paragraph saying, in effect,
"here is the file key, locked so that
holder of identity X can unwrap it."
That is the whole trick, and three
consequences fall out of it:

- **Several recipients cost almost
  nothing.** Wrapping a 16-byte key again
  is trivial, so one file can be locked
  to five people as easily as to one
  (chapter 7). Each named person unwraps
  the same file key with their own
  identity; the contents are encrypted
  only once.
- **The contents are never re-encrypted
  per person.** Adding someone later
  means a new wrapping — but age has no
  command to edit a header, so in
  practice you decrypt and re-encrypt
  (chapter 7 is honest about this).
- **A passphrase is a stanza too.** In
  passphrase mode the file key is wrapped
  with a key derived from your passphrase
  by **scrypt**, a deliberately slow
  derivation that makes guessing
  expensive (chapter 6).

## The header

Every age file begins with a header you
can read. The lab's first encrypted note
begins (transcript t02):

```text
age-encryption.org/v1
-> X25519 cFPcd8covvoQN1ZS3yPA2jrmf8HIljfI2Ol/IDQfyAE
q4D1hJW0JnwEgpimHG8peZIgeBNZaVmUuelHG9idcHc
```

The first line names the format and its
version. Then come the stanzas — one per
recipient — each starting with `->`, its
type (`X25519` here), and the wrapped
file key. A closing line carries a **MAC**,
a checksum over the header computed with
the file key itself, so a header cannot
be quietly rearranged: the first thing an
identity proves on opening is that the
header it was handed is the header that
was written. After the header come the
contents, encrypted in chunks of 64 KiB
so that even a huge file opens as a
stream, a piece at a time, rather than
all at once in memory.

You can read a header without any key.
`age-inspect` (chapter 10) reports the
stanza types and sizes — which is also a
limit worth knowing now: **anyone holding
a locked file can see how many recipients
it has and of what kinds**, though never
who they are or what is inside.

## Armoring

The locked file is binary — awkward in an
email body or a chat message. **Armor**
is the same bytes written as text:
base64 between two marker lines, exactly
as the lab produced in transcript t03:

```text
-----BEGIN AGE ENCRYPTED FILE-----
YWdlLWVuY3J5cHRpb24ub3JnL3YxCi0+IFgyNTUxOS...
-----END AGE ENCRYPTED FILE-----
```

Nothing about the security changes; it
is a coat, not a second lock. age reads
armored and binary files alike, and
chapter 4 produces both. The cost is
size: armor inflates a file by about a
third (age-inspect showed 161 bytes of
armor overhead on the lab's 53-byte
note), which is why backups use binary
and pasted messages use armor.

## The parts, in one list

- **Recipient** — a public key string;
  locks. Share it anywhere.
- **Identity** — a secret key, usually a
  small file from `age-keygen`; unlocks.
  Guard it like a house key.
- **File key** — random, per file, never
  seen by you; the thing actually
  encrypting the contents.
- **Stanza** — one wrapping of the file
  key, for one recipient, in the header.
- **Header** — format line, stanzas, and
  a MAC; readable by anyone.
- **Payload** — the contents, encrypted
  and tamper-sealed in 64 KiB chunks.
- **Armor** — the whole file as text,
  for places binary cannot go.
