# Installing age

By the end of this chapter age will be
installed on your machine and proven
working, with its version printed — the
check every later chapter assumes.

age is a single small program (plus its
two companions, `age-keygen` and
`age-inspect`) with no service to run and
nothing to configure. Installation is a
two-minute job on most systems, and
there are three honest routes.

## Route 1: your package manager

Most systems carry age in their standard
repositories, and this is the route to
try first:

```bash
# Debian / Ubuntu
sudo apt install age

# Fedora
sudo dnf install age

# Arch
sudo pacman -S age

# macOS, with Homebrew
brew install age

# Windows, with winget
winget install FiloSottile.age
```

Package versions trail the official
releases a little, which rarely matters:
the file format is stable, and a file
locked by age 1.0 opens in age 1.3 and
the other way round. What matters is
that all three tools arrive together.

## Route 2: the official release

The project's releases page (on GitHub,
under FiloSottile/age) publishes signed,
self-contained builds for Linux, macOS,
Windows and the BSDs. Download the
archive for your system, unpack it, and
you get a folder holding `age`,
`age-keygen`, `age-inspect` and several
`age-plugin-*` helpers. Put the folder
somewhere on your `PATH`, or run the
tools by their full path.

This is the route the lab used — the
build machine's package sources did not
carry age at all — and it is transcript
t00's machine: **age 1.3.2**, unpacked
from `age-v1.3.2-linux-amd64.tar.gz`.
Nothing was compiled and nothing was
installed system-wide; the tools ran
from their folder.

## Route 3: build from source

age is written in Go, and a Go toolchain
builds it in the usual way
(`go install filippo.io/age/cmd/...@latest`).
You need this route only if your system
has no package and no official build.
It is documented here, not run: the lab
had no reason to reach for it.

## The proof

Whichever route you took, finish with
the check:

```bash
age --version
age-keygen --version
```

Both should print the same version
number (the lab's printed `v1.3.2`).
Two more habits worth forming now:

- **Let `age-keygen` refuse to overwrite.**
  It will not replace an existing key
  file; that refusal has saved more keys
  than any backup. Chapter 5 has the
  rest of key care.
- **Notice where the tools are.**
  `which age` (or `where age` on Windows)
  tells you which copy you are running —
  the answer matters the day a script
  behaves differently from your shell
  (chapter 16).

No accounts were created, no daemon is
running, and nothing phones home. You
have three small tools. The next chapter
uses two of them.
