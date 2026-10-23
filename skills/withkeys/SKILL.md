---
name: withkeys
description: Run a command that needs API keys or passwords through
  withkeys, which takes them from pass. Use when a command needs a token,
  a password or an API key, or fails for lack of one.
---

# withkeys

README.md, two levels above this file, says where keys live, how a
project gets its own and how a program gets its keys: by a shim, or
`withkeys APP COMMAND`.

Never read, print or copy keys. When one is missing, tell the user the
command to add it, and let them type the value:

    pass insert withkeys/APP/VAR

"Can't decrypt" means the gpg agent is locked: ask the user to unlock it,
the way they do.
