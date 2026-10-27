# Troubleshooting

By the end of this chapter the failures
age can hand you will each have a name,
a cause and a next step. Nearly all of
them were produced on purpose by this
guide's lab, so each entry quotes the
real message.

## "no identity matched any of the recipients"

```text
age: error: no identity matched any of the recipients
```

The identities you offered with `-i` do
not correspond to any stanza in the
file. Causes, in order of embarrassment:

- The wrong key file: a spare, an old
  one, the one from the other machine.
  Print its recipient and compare with
  the recipient the file's sender
  intended: `age-keygen -y that-file`.
- The file was locked to your *SSH* key
  (chapter 9), and you are offering
  your age key — or the reverse.
- The file was genuinely never locked
  to you. This is the mechanism
  working, and transcript t09 is the
  lab doing it to itself with a third
  key. Nothing is broken, and nothing
  can be tuned to make it open.

## "incorrect passphrase"

```text
age: error: incorrect passphrase
```

Wrong phrase — or the right phrase
typed into the wrong file (chapter 6).
age retries nothing and writes nothing.
Check the obvious: keyboard layout,
a trailing space pasted along with the
phrase, and — if the passphrase came
from a manager — that you copied the
entry for *this* file.

## The file that is not an age file

Decryption fails at once, complaining
about the header, or `age-inspect`
declares it cannot parse the file.
Likely stories:

- **Clipped armor.** The file travelled
  as text and lost its first or last
  line — chat programs and web forms
  trim trailing lines. The
  `-----BEGIN AGE ENCRYPTED FILE-----`
  and matching `END` lines must both be
  present, complete. Ask for a resend
  as an attachment, or check the
  message's "show original" view.
- **A helpful editor touched it.** An
  armored file opened and re-saved by a
  word processor, or a binary file
  passed through a tool that "fixed"
  its line endings, is damaged goods.
  Re-transfer it as a file, untouched.
- **It was never age's.** A `.age`
  extension is a convention, not a
  promise; people rename things.
  `age-inspect` remains the honest
  first question.

## The payload fails its seal

A stanza unwraps, decryption starts,
and then fails with an authentication
error: the file changed after locking,
or rotted in storage, or a transfer cut
it short. There is no partial recovery
and no repair flag — by design, since
a tool that "repaired" sealed contents
would be a tool for tampering with
them. Go to another copy. If every copy
fails this way, the locking machine is
the suspect (memory and disk errors
corrupt before encryption too), and
chapter 10's rule — verify on the day
you make it — is the eulogy.

## The prompt that seems to hang

Decryption sits at "Enter passphrase:"
for a file you expected a key to open.
The file is passphrase-locked (a
passphrase stanza is tried when no
identity matches it — or the sender
used chapter 6 where you expected
chapter 4); or the identity file you
gave is an SSH key with its own
passphrase, and the prompt is for the
*key's* phrase, not the file's
(chapter 9). Either way age is waiting
for typing, in the terminal, possibly
behind another window.

## Two copies of age disagree

A script behaves differently from your
shell — an older age elsewhere on the
`PATH`, typically one from a package
beside your newer release build, and
the old one lacks `-pq` or
`age-inspect`. `which -a age` lists
every copy; chapter 3's version check
settles which one a command really
ran. Mixed versions still read each
other's ordinary files — the format is
stable — so this failure shows up at
the new features first.

## The passphrase prompt refuses a pipe

A script tries `echo phrase | age -p`
and age objects: passphrase encryption
wants a terminal, on purpose (chapter
11). The fix is not a flag but a
change of design — keys for unattended
work. The one sanctioned halfway house
is `age-plugin-batchpass`, shipped in
the release for driving plugins in
tests and scripts; read its warnings
before letting it anywhere near a real
secret, since it exists to make
passphrase handling scriptable, with
all the exposure that implies.
