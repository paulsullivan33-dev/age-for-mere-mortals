# SSH Keys as Recipients

By the end of this chapter you will be
able to lock a file to an SSH public
key — quite possibly one you already
have — and open it with the matching
SSH private key.

## Why this exists

**SSH** is the tool most technical
readers already use to log into other
machines, and it runs on key pairs of
its own. Rather than make you mint and
distribute a second key pair just for
files, age accepts an SSH public key as
a recipient, and the SSH private key as
an identity. The key you use to reach
your server can also be the key your
backups are locked to.

## Locking to an SSH key

An SSH public key is the one-line
contents of a `.pub` file:

```text
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIJIyuPavDgHWgB2f47+EouqtegtsquvqqBP+VSaBeEnG age-lab throwaway
```

Pass the whole line as the recipient:

```bash
age -r "ssh-ed25519 AAAA..." -o note.txt.age note.txt
```

The stanza in the header will be of
type `ssh-ed25519` instead of `X25519`,
but nothing else about the file
changes. The lab did exactly this with
a throwaway key it generated for the
purpose (`ssh-keygen -t ed25519`),
transcript t08.

## Opening with an SSH key

```bash
age -d -i lab-ssh-key -o note.txt note.txt.age
```

The identity is the SSH *private* key
file — the one without the `.pub`. If
that private key is itself protected by
a passphrase (a good SSH habit), age
prompts for it at decryption time, the
same way SSH does.

## The fine print

- **Ed25519 keys are the reliable
  case.** age's SSH support is built
  around them. RSA SSH keys are also
  accepted as recipients in current
  versions — encryption to them works,
  though their stanzas are large. The
  lab verified Ed25519 end to end and
  describes RSA from the documentation;
  if your only SSH key is an old RSA
  one, test the round trip before
  trusting it (chapter 10's method,
  always).
- **Old key types are not accepted.**
  DSA keys, and keys in some very old
  formats, will be refused with an
  error rather than silently mishandled.
- **The house-key rule does not
  soften.** Your SSH private key just
  became an identity in chapter 5's
  sense: everything locked to it opens
  with it. If it was casually copied to
  six machines for convenience, it now
  guards your files from all six
  places too. For many people that is
  an argument for a separate age key
  pair after all — one key per job,
  each guarded to the standard of what
  it opens. The tool allows the reuse;
  the judgement is yours.
