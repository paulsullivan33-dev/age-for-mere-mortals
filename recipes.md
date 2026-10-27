# Recipes

Each recipe is a complete job. All of
them end with a verification, because
that is the house rule (chapter 10).

## A backup that leaves the house

**Job:** the family photo archive goes
to a drive at a relative's house, and
to cloud storage you do not control.

```bash
tar -cf - photos/ | age -r age1mine... -r age1spare... \
    -o photos-2026.tar.age
sha256sum photos-2026.tar.age   # record it with the archive
```

Locked to two recipients: yours, and
the spare identity kept on paper
(chapter 11). The drive and the cloud
now hold only a locked file and its
hash. Verify on arrival by streaming
the decryption's hash (chapter 10)
before the original drive is reused.

## A secret inside an email

**Job:** one document, one person, and
email is the only road.

```bash
age --armor -r age1theirs... -o statement.age.asc statement.pdf
```

Paste the armored block into the
message body. The attachment route is
equally fine unarmored — armor earns
its keep when the file must travel as
text. Warn the recipient about chapter
16's trap: chat and mail programs that
clip the first or last line of the
block make it unreadable; the
`BEGIN`/`END` lines must both survive.

And remind yourself, as the sender:
you cannot open it after sending,
unless your own recipient was also on
the command. If you need a copy you can
open, lock a second one to yourself.

## A shared team drop file

**Job:** a small team's shared notes,
readable by the whole team, updated by
whoever touched it last.

Keep the team's recipients file under
version control (chapter 8), and lock
to it every time:

```bash
age -R team-recipients.txt -o shared-notes.txt.age shared-notes.txt
```

Rotation discipline: whoever edits,
re-locks — to the file as it stands
*that day*, not as it stood when they
joined. The stale-copy failure (an old
locked file whose recipient list
predates a new member) is social, not
technical, and the recipients file's
commit history is how you spot it.

## The leaving handover

**Job:** a member of the team is
leaving, kindly or otherwise.

1. Remove their line from the
   recipients file; commit the change.
2. Re-lock every shared file they
   could open, from plaintext or from
   an identity that stays (yours),
   against the new file.
3. Replace what they knew: shared
   passphrases (chapter 6) that the
   files relied on anywhere else.
4. Accept the limit chapter 7 stated:
   plaintext they already took is
   theirs. The handover protects the
   future, and doing it the day they
   leave — not the month after — is
   the entire trick.

## A key's own emergency envelope

**Job:** the recovery copy of your
identity, for the drawer.

Print the identity file. Beside it,
write the recipient it belongs to
(the `age-keygen -y` output), the date,
and one line saying what it opens
("house backups, 2026 onwards"). Seal
it if the drawer is shared. Then —
once, the day you file it — take it
out and open a test file with the
printed text typed back in. Paper that
has never been re-typed is paper you
are hoping about (chapter 5), and a
single transposed character is a lost
archive discovered at the worst
possible moment.
