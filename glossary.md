# Glossary

**age** — the tool itself (Actually
Good Encryption), by Filippo Valsorda
and collaborators; locks files to named
recipients (chapter 1).

**age-inspect** — the companion tool
that reads a locked file's header and
reports stanza types and sizes without
decrypting (chapter 10).

**age-keygen** — the companion tool that
generates key pairs, and with `-y`
prints the recipient inside an identity
file (chapters 4, 5).

**Armor** — a locked file written as
base64 text between `BEGIN`/`END AGE
ENCRYPTED FILE` lines, for places
binary cannot go (chapter 2). The flag
is `--armor` or `-a`.

**ChaCha20-Poly1305** — the
authenticated cipher that encrypts the
payload with the file key; the
Poly1305 seal is why tampering is
detected on opening (chapter 2).

**File key** — a fresh random 16-byte
key generated for each file, which
actually encrypts the contents; it is
wrapped once per recipient into
stanzas (chapter 2).

**Header** — the opening, readable part
of every age file: the format line
`age-encryption.org/v1`, the recipient
stanzas, and a MAC over the header
(chapter 2).

**Identity** — a secret key; the
unlocking half of a pair, usually a
small file from `age-keygen` beginning
`AGE-SECRET-KEY-1`. A house key
(chapters 1, 5).

**MAC** — message authentication code;
here, a checksum over the header keyed
by the file key, proving the header was
not rearranged (chapter 2).

**ML-KEM-768** — the standardised
post-quantum key-encapsulation method
combined with X25519 in age's hybrid
post-quantum keys (chapter 12).

**Payload** — the encrypted contents
after the header, sealed and chunked
at 64 KiB (chapter 2).

**Passphrase mode** — locking a file
with a passphrase instead of keys
(`age -p`); the file key is wrapped
with a scrypt-derived key (chapter 6).

**Plugin** — a separate program
(`age-plugin-*`) that teaches age a new
stanza type, such as one backed by a
hardware security key (chapter 12).

**Post-quantum (PQ) key** — a hybrid
key pair from `age-keygen -pq`;
recipients begin `age1pq`, identities
`AGE-SECRET-KEY-PQ-1` (chapter 12).

**Recipient** — a public key; the
locking half of a pair, an `age1...`
string safe to publish (chapter 2).

**Recipients file** — a text file of
recipients, one per line, used with
`-R` to lock for a whole group at once
(chapter 8).

**Round trip** — encrypting and then
decrypting the same file and comparing
the result with the original; the
verification habit of the whole guide
(chapters 4, 10).

**scrypt** — the deliberately slow
key-derivation function that turns a
passphrase into a wrapping key, taxing
every guess (chapter 6).

**SSH key** — a key pair from the SSH
world; age accepts an Ed25519 SSH
public key as a recipient and the
private key as an identity
(chapter 9).

**Stanza** — one wrapping of the file
key, for one recipient, written in the
header and labelled with its type —
`X25519`, `scrypt`, `ssh-ed25519`,
`mlkem768x25519` (chapter 2).

**X25519** — the key-agreement scheme
under age's ordinary key pairs
(chapter 2).
