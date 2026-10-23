---
name: withkeys
description: Run a command that needs API keys or passwords through
  withkeys, which takes them from pass. Use when a command needs a token,
  a password or an API key, or fails for lack of one.
---

# withkeys

A program with a shim in `~/.local/shims` gets its keys on its own: run it
by name. For one without a shim:

    withkeys APP COMMAND [ARGUMENTS]

Keys are `~/.password-store/withkeys/APP/VAR.gpg`, set as VAR. A working
copy with `git config withkeys.profile NAME` takes them from
`withkeys/APP/NAME/` when it exists.

Never read, print or copy keys. When one is missing, tell the user the
command to add it, and let them type the value:

    pass insert withkeys/APP/VAR

"Can't decrypt" means the gpg agent is locked: ask the user to unlock it,
the way they do.
