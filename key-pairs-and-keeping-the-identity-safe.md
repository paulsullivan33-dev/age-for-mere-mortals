# Key Pairs — Keeping the Identity Safe

By the end of this chapter your identity
file will have a deliberate home, a
backup of its own, and an honest answer
to the two questions that matter about
it: what happens if it leaks, and what
happens if it is lost.

## What the identity file is

It is three lines of text: a creation
date, the public key as a comment, and
the secret itself — 20-odd words' worth
of characters beginning
`AGE-SECRET-KEY-1`. That smallness is
the point and the danger. It can be
copied by anything that can read it:
a backup program doing its job, a sync
folder doing its job, malware doing
its. Whoever reads it can open every
file ever locked to its recipient, and
there is no way to lock them out again
short of re-encrypting everything to a
fresh key (chapter 13's handover
recipe).

age helps where it can. The identity
file is created readable only by its
owner, and `age-keygen` flatly refuses
to overwrite an existing key file. The
rest is up to you.

## A deliberate home

Good homes for an identity file, in
roughly the order this guide trusts
them:

- **A folder only your account can
  read**, on a machine whose disk is
  itself encrypted — the ordinary case,
  and good enough for most people.
  Permissions should read `-rw-------`;
  check with `ls -l`, fix with
  `chmod 600 my-key.txt`.
- **On paper, in a drawer.** The
  identity is short enough to print.
  Paper does not sync to anything, is
  immune to malware, and burns only in
  fires your house key would not
  survive either. For a key that guards
  long-term backups, a printed copy is
  a serious option, not a quaint one.
- **Inside a password manager's file
  vault** — the identity protected by
  the manager's own encryption at rest.
  This guide's sister volume on
  KeePassXC covers that pattern.

Bad homes: a sync folder that copies it
to three clouds and a phone; an email
to yourself ("so I can always get it"
— so can anyone who gets your mailbox);
the same unlocked drive as the backups
it protects, where one theft takes
both the lock and the key.

## The key needs a backup too

Rule 3 from chapter 1: a lost identity
means lost files. So the identity
itself needs the thing it exists to
protect — a copy somewhere else. Keep
at least two copies in at least two
places, under the same discipline as
the original: a printed copy at home
and the file on the encrypted machine,
for example. And once a year, when you
test your backups (chapter 11), test
that a copy of the identity still opens
a test file: `age-keygen -y` reads an
identity file and prints its recipient,
which proves the file is readable and
whole without decrypting anything:

```bash
age-keygen -y my-key.txt
```

The lab ran this on its key in
transcript t11; the printed recipient
must match the one you encrypted to.

## If it leaks

Suspect a leak — the file was on a
stolen laptop, in a folder that got
shared — and the response is brisk and
unpleasant, and there is no way to make
it smaller:

1. Generate a **new key pair**.
2. **Re-encrypt every file** that
   matters from the old recipient to the
   new one (decrypt with the old
   identity, encrypt to the new
   recipient). Files you cannot
   re-encrypt — copies already sent —
   must be treated as readable by
   whoever took the key.
3. Hand out the **new recipient**
   everywhere the old one was used.

Leaked is bad. It is still better than
lost, because leaked-you can act.

## If it is lost

There is no procedure. No reset, no
recovery key unless you made one in
advance (a second recipient on your
important files — a spare identity kept
on paper — is exactly a recovery key;
chapter 7 shows how painless adding one
is at encryption time). This is the
cliff edge of the whole design, stated
in chapter 1 and repeated here because
this chapter is where you prevent it:
**two copies, two places, before real
files depend on the key.**

One last distinction. The *recipient* —
the `age1...` string — needs no
protection at all, and the more places
it is written down, the better: it is
how people lock files *for* you. Guard
the identity. Publish the recipient.
Confusing the two, in either direction,
is the only real way to misuse this
tool.
