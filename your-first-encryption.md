# Your First Encryption

By the end of this chapter you will have
made a key pair, locked a note to it,
unlocked the note, and proven the round
trip byte-for-byte — the whole of age in
five minutes. Everything later is this
chapter with variations.

Work in a scratch folder with files you
can afford to lose. The lab's file is a
one-line note; yours can be any small
text file.

## Step 1: make a key pair

```bash
age-keygen -o my-key.txt
```

`age-keygen` prints the public key and
writes the identity file:

```text
Public key: age1jf2fmygfh44vq0zkvdfh5jmtaac9n5t8txk28fz6044hak3fe4fsn9agud
```

(The lab's key, from transcript t01 —
yours will differ, and that is the
point.) The file `my-key.txt` holds a
creation date, the same public key as a
comment, and the secret line beginning
`AGE-SECRET-KEY-1`. Two facts to fix in
mind now: the `age1...` string is the
**recipient**, safe to give anyone; the
file is the **identity**, a house key.
Chapter 5 covers its care. For this
chapter, keep it beside the note.

## Step 2: write something worth locking

```bash
echo "The backup drive lives in the blue box on the shelf." > note.txt
```

## Step 3: lock it

```bash
age -r age1jf2f... -o note.txt.age note.txt
```

Replace the recipient with your own
public key. The flags: `-r` names a
recipient, `-o` names the output. age
adds no extension by itself; `.age` is a
convention this guide follows so locked
files are recognisable at a glance. The
result, `note.txt.age`, is binary. Its
first bytes are the header from chapter
2 — run `head -c 120 note.txt.age` and
you will see `age-encryption.org/v1` and
an `X25519` stanza looking back at you.

## Step 4: unlock it

```bash
age --decrypt -i my-key.txt -o note-back.txt note.txt.age
```

(`-d` is the short form of `--decrypt`,
and either works.) No fanfare: age reads
the identity file, finds the stanza it
can unwrap, and writes the plaintext.

## Step 5: prove it

```bash
diff note.txt note-back.txt
```

Silence is success: the files are
identical, byte for byte. The lab ran
exactly these commands — transcript t02
— and every variation in the next seven
chapters ends with this same check,
because a round trip you have not
verified is a hope, not a backup.

## The same file, as text

For a locked file that must travel
inside a message, add `--armor` (or
`-a`) when locking:

```bash
age --armor -r age1jf2f... -o note.txt.age.asc note.txt
```

You get the `BEGIN AGE ENCRYPTED FILE`
block from chapter 2, ready to paste.
Unlocking is unchanged — age recognises
the armor on sight (transcript t03).

## What you have, and what you have not

You have a file that can sit on any
drive, in any inbox, in any country,
readable by exactly one key: yours. You
have not protected the note still
sitting beside it in plain text, the
filename (`note.txt.age` announces what
it was), or yourself against losing
`my-key.txt`. The first two are chapter
15's business. The third is the next
chapter's.
