# Plugins and Hardware Keys

By the end of this chapter you will
understand what age's plugin system is
for, what it would mean to hold your
identity inside a security key instead
of a file, and what the post-quantum
keys in this build change. Read the
labels as you go: the post-quantum half
of this chapter was **run** by the lab;
the hardware half is **described from
the projects' documentation** — the lab
had no security key to test with, and
this guide does not pretend otherwise.

## Why plugins exist

Chapter 2's stanzas are types, and the
list of types is not closed. A **plugin**
is a separate small program that teaches
age a new stanza type: age hands the
plugin the file key to wrap (or a stanza
to unwrap), and the plugin does whatever
its kind of key requires — ask a human
to touch a security key, talk to a
smartcard, call a cloud key service.
Plugin recipients are recognisable:
their strings name the plugin inside
them, in the shape
`age1<pluginname>1...`, and a file locked
to one can only be opened on a machine
where that plugin is installed.

The release the lab ran ships three
plugin programs beside age itself (their
names are in transcript t00's folder
listing): `age-plugin-batchpass`,
`age-plugin-tag` and `age-plugin-pq`.
They are infrastructure more than
end-user tools — batchpass exists so
other plugins can be tested and driven
in scripts — but their presence makes
the architecture concrete: the plugin
system is not a promise, it is in the
box.

## Hardware keys, as documented

Two third-party plugins carry the idea
furthest, and both are documented-only
in this guide:

- **`age-plugin-yubikey`** lets a
  YubiKey — a small USB security token
  with a PIV smartcard inside — hold
  the identity. Locking works anywhere;
  opening asks for the physical key to
  be present and touched, and a PIN if
  one is set. The identity cannot be
  copied out of the token, which retires
  chapter 5's whole class of worries
  about a key file being silently read.
- **`age-plugin-fido2`** does the same
  for any FIDO2 security key — the kind
  sold for web logins — deriving and
  holding age identities on the token.

What changes, and what does not, if you
adopt one (all of this follows from the
design, and is why the chapter is
labelled *carefully*):

- **Theft-resistance goes up.** A file
  can be copied in a second; a token
  must be physically taken, and its
  absence is noticeable.
- **Loss becomes a sharper cliff.** A
  token that dies or walks away takes
  its identity with it, perfectly and
  finally. The planning answer is the
  same as chapter 11's: every important
  file locked to **two** recipients —
  the token's, and a spare kept
  elsewhere.
- **The stanza type travels with the
  file.** A file locked to a YubiKey
  recipient says so in its header
  (`age-inspect` will show the plugin
  type to anyone) and opens only where
  that plugin and that token are.

## Post-quantum keys: run, not read

age 1.3 adds a second native key type,
and this half the lab did run
(transcript t12):

```bash
age-keygen -pq -o pq-key.txt
```

The `-pq` flag generates a **hybrid
post-quantum** pair: the usual X25519
combined with **ML-KEM-768**, a
standardised post-quantum key-encapsulation
method. Hybrid means both halves must
hold: the classical half protects
against today's attacks, the ML-KEM
half against a future quantum computer
recording files now to open later —
the "harvest now, decrypt later" worry
that matters for archives meant to stay
secret for decades.

What you will notice in practice:

- The recipient string begins `age1pq`
  and is *long* — over two thousand
  characters, against 62 for X25519.
  Recipients files (chapter 8) hold
  them fine; pasting one into a chat
  message is a small ordeal.
- The identity line begins
  `AGE-SECRET-KEY-PQ-1`.
- Headers grow: 1,627 bytes against
  168 for the same note under X25519,
  per `age-inspect` in transcript t12.
  On real files this is nothing; it is
  the honest price of the larger sums.
- The round trip is otherwise
  identical: same commands, same flags,
  verified byte-for-byte by the lab.

Should you switch? For files whose
secrecy must outlive a decade, the
hybrid key is the prudent default and
costs nothing but longer strings. For
tonight's note to the accountant,
X25519 remains entirely respectable.
Either way the choice is per key pair,
made once at `age-keygen` time — and,
per the help text's own remark, the
post-quantum pair "might become the
default in the future."
