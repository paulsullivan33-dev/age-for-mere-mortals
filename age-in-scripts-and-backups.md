# age in Scripts and Backups

By the end of this chapter age will be
working unattended for you: archives
locked as they are made, restores
verified as they land, and the failure
of either announced by an exit code
your script can act on.

## age is built for pipes

Both directions stream. Locking needs
no plaintext file on disk at all:

```bash
tar -cf - photos/ | age -r age1... -o photos.tar.age
```

and restoring needs no decrypted file
either:

```bash
age -d -i my-key.txt photos.tar.age | tar -xf -
```

The lab ran both directions with a tar
archive and listed the restored
contents as proof (transcript t11). For
a backup tool this is the whole game:
the archive is born locked, travels
locked, and is plaintext only in memory
at the moment of restore.

## The scripting contract

- **Exit codes tell the truth.** 0 is
  success; 1 covers every failure this
  guide has shown — wrong identity,
  wrong passphrase, damaged payload.
  age writes no output file on failure,
  so a script can test the exit code
  and trust the result. Check it:
  `set -e` in shell, or the equivalent,
  because an unchecked failure is how a
  backup job "succeeds" for a year
  while writing nothing.
- **Errors go to standard error, data
  to standard output.** A pipe never
  mixes age's complaints into your
  plaintext.
- **Passphrases and scripts do not
  mix.** `age -p` demands a terminal for
  its prompt and refuses to read a
  passphrase from a pipe. That refusal
  is a feature: a passphrase sitting in
  a script is a key sitting in a file.
  Unattended jobs use key files (with
  chapter 5's permissions), or fetch an
  identity at run time and hand it to
  `-i -` on standard input — the lab's
  transcript t13 shows an identity
  piped in exactly that way.
- **age will overwrite its `-o`
  target without a murmur.** The lab
  encrypted twice to the same path and
  the second run simply replaced the
  first (transcript t13) — unlike
  `age-keygen`, which refuses to
  overwrite a key file. So a script
  must guard its own outputs: check
  before writing, keep the previous
  archive until the new one is
  verified, and never, ever point
  `-o` at the run's input file.

## A backup shape that works

The pattern this guide recommends, in
full:

1. The backup script tars the data,
   pipes it through age locked to **two
   recipients**: yours, and a spare
   identity kept on paper (chapter 5's
   recovery key — the day the main key
   is lost, this is the chapter you
   will be glad you read).
2. The locked archive is copied
   off-site. In transit and at rest it
   is a locked file; the storage does
   not need to be trusted.
3. **Monthly, the restore is tested**:
   decrypt to a scratch space, hash or
   list the contents, compare with
   expectations (chapter 10's method).
   An untested backup is a hypothesis.
4. The identities' own backups are
   checked on the same schedule:
   `age-keygen -y` on each copy, the
   recipients compared against the
   recipients file the backups are
   locked to.

## Rotation and retention

Old backups under an old key are a
decision, not an accident. When a key
is retired (chapter 5's handover),
either re-encrypt the archives you
still need, or keep the old identity —
guarded, labelled, on paper if nowhere
else — for exactly as long as those
archives live. Deleting a key while
its backups survive is scheduling a
loss. The retention rule and the key
rule are one rule: **an archive and
its key retire together, or not at
all.**
