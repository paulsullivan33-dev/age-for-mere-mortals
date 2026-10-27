# Encrypting for Several People

By the end of this chapter you will be
able to lock one file so that any of
several people — each with their own
key, and only their own key — can open
it.

## Repeat the flag

Chapter 2 promised this would be easy,
and it is. One `-r` per person:

```bash
age -r age1ana... -r age1ben... -o rota.txt.age rota.txt
```

The file gets one stanza per recipient,
each wrapping the same file key. Ana
opens it with Ana's identity; Ben with
Ben's. Neither gains anything about the
other's key, and either can be you:
including your own recipient on every
shared file is a habit worth forming
before you need it (chapter 13 turns it
into the leaving-handover recipe).

The lab encrypted its rota file to two
key pairs and opened it as each holder
in turn — transcript t06. Both round
trips verified byte-for-byte, and the
two identity files never met.

## What each person needs

Only this: the locked file, and their
own identity file. Not the other
recipients' keys, public or secret, and
not the sender's identity — the sender
cannot open the file again either,
unless the sender's own recipient is
among the `-r` flags. Read that again
if it surprises you: **locking a file
to someone else locks you out**, unless
you name yourself too. More than one
shared file has been lost to exactly
that moment of generosity.

## The honest limits of sharing

- **Anyone who can open it can copy
  it.** A recipient who decrypts a file
  has the plaintext, forever, in
  whatever form they keep it. age
  controls the locked copies only.
- **Adding a person means
  re-encrypting.** There is no command
  to add a stanza to an existing file,
  even if you hold an identity that
  opens it. Keep the plaintext (or an
  identity that opens the file) and
  produce a fresh locked file with the
  longer recipient list. Plan lists
  accordingly: it is far easier to lock
  generously at the start than to widen
  a file later.
- **Removing a person means the same,
  plus suspicion.** Re-encrypt without
  them and future files are safe; copies
  they already hold, and plaintext they
  already took, are beyond reach
  (chapter 5's leak procedure).
- **The head count shows.** Anyone with
  the locked file can run `age-inspect`
  and count the stanzas (chapter 10).
  They cannot learn who the recipients
  are — only how many, and of what
  kinds.

## A word on group keys

The tempting shortcut — one key pair
for the team, identity file shared
round — dissolves everything chapter 5
built. A shared identity cannot leak
in part or be revoked in part; when
(not if) it spreads too far, every file
ever locked to it is open, and there is
no list of who has it. Per-person
recipients cost one extra `-r` flag
each. Pay it. Where the flag list grows
long enough to be unwieldy, the next
chapter files it.
