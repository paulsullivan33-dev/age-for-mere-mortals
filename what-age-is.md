# What age Is

By the end of this chapter you will be
able to say what age does, what its three
small tools are for, and the three rules
that govern every later chapter.

Some files are harmless anywhere. Others
— a scan of a passport, the family budget,
a backup of everything — should only ever
be read by specific people. The problem
is that files travel: into email, onto
USB sticks, up to storage you do not
control. **age** solves the travelling
part. It takes a file and locks it to
named **recipients**, producing a new file
that anyone may carry or store but only
the named people can open.

The locking is **public-key encryption**,
and the idea is older than age itself.
Each person has a pair of keys that fit
together: a **public key**, the
**recipient**, which locks, and a **secret
key**, the **identity**, which unlocks.
The public key can be printed on a web
page; it only locks. The secret key is
never shared; it is the only thing that
opens what its public half locked. age's
contribution is not the idea — it is
stripping everything else away. No
accounts, no key servers, no web of trust,
no settings file full of ciphers to
choose. One job, done one way, the same
way every time.

age is also deliberately small. The whole
toolbox in this guide is:

- **`age`** — locks and unlocks files.
- **`age-keygen`** — makes a key pair, and
  can read a secret key file back and tell
  you its public half.
- **`age-inspect`** — reads a locked file's
  outside: what kind of recipients it was
  locked to, and how big each part is,
  without opening it.

That is the surface. The format they all
speak is a public specification
(age-encryption.org), written down so
that other programs can lock and unlock
the same files — and they do: age files
can be opened by independent
implementations in several languages,
which matters for chapter 15's subject,
the long life of backups.

## The rules

Three rules govern this guide, and they
are part of the tool, not decoration:

1. **Encrypt your own files.** age is for
   protecting what is yours, or what the
   owner has plainly asked you to protect.
   Every example here is the lab locking
   its own notes to its own keys.
2. **Keep your identity key like a house
   key.** The secret key file is small,
   plain text, and total: whoever copies
   it can open everything ever locked to
   its public half, past and future. You
   would not email a house key. Chapter 5
   is about where this file lives.
3. **A lost identity means lost files.**
   There is no recovery, no reset link, no
   support desk. If the only copy of a
   secret key is gone, the files locked to
   it are gone with it — not damaged, not
   degraded: mathematically out of reach.
   Plan for this before it is urgent,
   which chapter 5 also covers.

One more habit follows from the rules and
runs through the whole guide: **verify
every round trip.** A locked file you
have never test-opened is a hope, not a
backup. The lab decrypted everything it
encrypted and compared the result
byte-for-byte, and chapter 10 makes a
method of it.

## Where age fits

If you have met file encryption before,
it was probably one of the neighbours in
chapter 14: GnuPG's thicket of key types
and trust models, OpenSSL's spell-book of
flags, or the password box in an archive
program. age is what you reach for when
the job is simply "lock this file to
these people" and you want the tool out
of the way: one key pair per person, one
command to lock, one to unlock, and a
format a second program can check your
work against. What it refuses to do —
signatures, key directories, encrypting
to someone whose key you cannot already
name — is listed just as plainly in
chapter 15.
