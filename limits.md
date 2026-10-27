# Limits

By the end of this chapter you will be
able to list what age will not do —
before one of these limits lists itself
for you, at a worse moment.

## No signatures

age proves one thing on opening: that
the file was not altered after locking,
sealed to the file key. It does **not**
prove who locked it. Anyone with your
recipient — which is anyone, it is a
public key — can lock a file "to you"
that you never saw. If a workflow needs
*this came from Ana*, age alone cannot
say so; that is signature territory,
and chapter 14's GnuPG is the
neighbour who lives there. Do not let a
smooth decryption whisper an
authorship it never checked.

## No key discovery, no directory

age will not find anyone's key for
you. There is no server to look a name
up on, no account, no "send to a
person" — there is a string you were
given, or there is nothing. This is a
design stance, and mostly a virtue
(nothing to breach, nothing to shut
down), but it means the social work —
collecting true keys, checking them,
keeping the recipients file honest
(chapter 8) — is yours, done by hand,
forever.

## The outside of the file still shows

A locked file keeps secrets about its
contents, not about itself. Visible to
anyone holding it:

- **Its size**, near enough (the
  payload is the plaintext's size plus
  a small, fixed overhead — age-inspect
  in transcript t04 itemises it).
- **Its recipient count and stanza
  types** — how many keys, of what
  kinds (chapter 10).
- **Its name**, if you keep the
  helpful convention of
  `tax-return.pdf.age`. age does not
  rename or hide the file around the
  contents. If the name itself is a
  secret, rename it yourself, to
  something that is not.

## The lost-key cliff

Stated for the third time, because this
chapter is its home: lose the only copy
of an identity, and its files are
unrecoverable — not by age's author,
not by a bigger computer, not by
anyone. It is the same mathematics that
makes the protection real. Mitigations
exist (spare recipients, printed keys —
chapters 5 and 11) and all of them are
things you do *before*.

The twin edge: age cannot stop a
recipient from keeping, copying or
publishing the plaintext they lawfully
opened. Encryption ends at decryption.
Anything past that is trust, contracts
and law — other tools than this one.

## Passphrases are the soft underbelly

Chapter 6's verdict belongs in this
list too: a passphrase-locked file's
strength is the passphrase's strength,
and human-chosen passphrases fall to
patient guessing that scrypt can only
slow. The file format is equally strong
either way; the secret is not.

## Small print

- **One file at a time.** age locks
  files, not folders or whole disks;
  tar (or your archive tool) makes the
  bundle, age locks it (chapter 11).
  Full-disk encryption is a different
  layer for a different threat.
- **No editing in place.** Adding or
  removing a recipient means a fresh
  encryption (chapter 7).
- **Young format, deliberately.**
  Version 1 of the format is what
  exists; the spec is written so other
  implementations can share it, and
  they do — but do not expect the
  thirty-year institutional weight
  behind OpenPGP. For archives meant
  to outlive decades, keep a copy of
  the age program itself beside them;
  chapter 12's post-quantum keys are
  the other half of that prudence.
