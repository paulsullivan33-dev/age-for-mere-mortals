# The Neighbours

By the end of this chapter you will be
able to say what each of age's three
neighbours does that age doesn't — and
when reaching past age for one of them
is the right call. These are
characterisations, not benchmarks: each
tool is described at its honest best.

## GnuPG

GnuPG (the GNU Privacy Guard, `gpg`) is
the elder of this street: three decades
of OpenPGP. It does everything age does
and a great deal more — **signatures**
(age has none at all), keys with
subkeys and expiry dates and revocation
certificates, a web of trust for
deciding whose key is really whose,
key servers to fetch keys from, and
integration with email clients that
grew up around it.

That power is also its weight. A GnuPG
key is a small bureaucracy: primary
keys, subkeys, user IDs, trust levels,
and a configuration surface large
enough that "which cipher did it
actually use" is a real question with a
non-obvious answer. age exists because
most file-locking jobs need none of
that: if your job is "lock this file
to these people," GnuPG is a cathedral
where a shed was asked for.

Reach for GnuPG when the job includes
*signing* — proving a file came from
you — when you live in email that
expects OpenPGP, or when the other
party's world is already built on it.
Stay with age when the job is locking
and unlocking files among people who
can hand each other a key string.

## OpenSSL

OpenSSL is not really a file-encryption
tool at all; it is the cryptography
library half the internet is built on,
with a command line that exposes its
plumbing. Its `enc` command will
encrypt a file with a passphrase and a
cipher you name — and there is the
characteristic difference: you name it.
Algorithms, key-derivation settings,
salt behaviour and padding are yours
to choose and yours to get wrong, and
its defaults have changed meaningfully
across versions, so a command copied
from an old page may quietly do
something weaker than its author
intended. OpenSSL also has no
recipient/key-pair mode for files in
any simple sense: public-key operations
exist at the level of individual
commands on keys and certificates,
suited to building systems, not to
locking a note.

Reach for OpenSSL when you are working
with certificates and TLS (its home
ground, and this series' OpenSSL guide
covers it), or when a procedure
genuinely specifies it. For locking a
file to a person, age's single opinion
is safer than OpenSSL's forty options.

## 7-Zip encryption

The archive program 7-Zip (and its
`7zz` command-line form) encrypts
archives with AES-256 under a
passphrase, and optionally encrypts the
archive's file list too, so even the
names inside are hidden — a trick age
does not do. For the very common job
"bundle these files and put a password
on the bundle, for a person with no
special tools," it is genuinely the
right reach: recipients everywhere have
something that opens `.7z`.

Its limits are chapter 6's limits in
uniform: passphrase only, no key pairs,
no per-person recipients, no way to
add or remove a person without
re-sharing a secret. And its strength
rides entirely on the passphrase, with
none of scrypt's ceremony explained to
the person choosing it.

Reach for 7-Zip when the other end is
a person, not a key. Reach for age
when the archive is a backup that must
stay locked for years, to named people,
with keys you can rotate.

## The short version

Signatures and the OpenPGP world:
GnuPG. Certificates and TLS plumbing:
OpenSSL. A password on a bundle for
anyone: 7-Zip. A file locked to named
people, with nothing else in the way:
the tool this guide is about.
