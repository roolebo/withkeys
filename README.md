# withkeys

Runs a command with API keys from [pass], as environment variables. Keys
stay encrypted on disk and reach only the command that needs them.

    withkeys APP COMMAND [ARGUMENTS]

Keys live in `~/.password-store/withkeys/APP/VAR.gpg`; the first line of
each is the value of VAR. Add one with:

    pass insert withkeys/APP/VAR

A symlink named after a program runs it with its keys, from any shell or
tool that calls it:

    ln -s ~/.local/bin/withkeys ~/.local/shims/APP

`~/.local/shims` goes first in PATH; withkeys finds the real APP in PATH
without it.

Projects can have their own keys. In a git working copy with a profile:

    git config withkeys.profile NAME

withkeys takes APP's keys from `withkeys/APP/NAME/` when it exists, else
from `withkeys/APP/`.

withkeys never prompts: unlock the gpg agent beforehand.

[pass]: https://www.passwordstore.org/
