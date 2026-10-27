# Recipients Files

By the end of this chapter you will keep
a group's public keys in one plain file,
lock files to the whole group with one
flag, and know why that file is safe to
share, publish and commit.

## The file

A **recipients file** is one public key
per line, with `#` comments allowed.
The lab's, from transcript t07:

```text
# The rota team - public keys only, safe to share and to commit
# Ana (played by the Alice key in this lab)
age1jf2fmygfh44vq0zkvdfh5jmtaac9n5t8txk28fz6044hak3fe4fsn9agud
# Ben (played by the Bob key)
age1ugrvga9y0mmdfv6kz94d3vlpw0yd7npdmheq9rp5e8u4klp8av2schz5kv
```

Lock to everyone in it with `-R`:

```bash
age -R team-recipients.txt -o rota.txt.age rota.txt
```

`-R` can be repeated for several files,
and mixed with `-r` for one-off extra
names. Opening is unchanged: each
person uses their own identity, exactly
as in chapter 7 — the lab opened the
`-R` file as Ben, and the round trip
verified.

## Why this file is safe to spread

It contains only recipients — the
locking halves. Chapter 5's rule:
publish the recipient, guard the
identity. A recipients file is
recipients in bulk, so it can live in a
shared folder, a team wiki, or a version
control repository, and leaking it
leaks nothing but the knowledge of who
is on the team.

The mirror-image warning bears saying
out loud, because the filenames are
similar: an **identity file** is also
one-key-per-line text (with comment
lines) — and chapter 2 showed age reads
identity files the same way. Never let
an identity file drift into the folder
where recipients files live. age will
happily encrypt *to* the public keys in
a file given with `-R` — and the day
someone puts a secret file there and
commits it, the handover in chapter 5
is the only way back.

## Keeping the file true

A recipients file is only as good as
its provenance. A substituted public
key in the list means files locked to a
stranger — age cannot tell Ana's real
key from an attacker's line pasted in
its place, because keys carry no names;
the comments are just comments. So:

- Collect keys from their owners by a
  channel you already trust, and check
  a few characters aloud or on screen
  the first time.
- Change the file deliberately, in the
  open: a commit history on a recipients
  file is an audit trail of who was
  added and removed, and when.
- When someone leaves, remove their
  line *and* re-encrypt what they could
  open (chapter 7 is blunt about what
  removal does and does not do).

`age-keygen -y` (chapter 5) prints the
recipient inside any identity file —
the quick way for a new member to read
you their own line for the file,
straight from their key, with no
transcription errors in the middle
(transcript t11).
