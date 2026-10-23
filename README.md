# withkeys: Your keys without headache

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
from `withkeys/APP/`. For example:

    ~/.password-store/withkeys/
    ├── rv/
    │   └── ANTHROPIC_API_KEY.gpg
    └── memex-gerrit/
        ├── GERRIT_USER.gpg          keys outside any profile
        ├── GERRIT_PASSWORD.gpg
        └── lustre/
            ├── GERRIT_USER.gpg      keys of profile lustre
            └── GERRIT_PASSWORD.gpg

## Unlocking

withkeys never prompts: unlock the gpg agent beforehand. One way, in
`~/.zshrc` or `~/.bashrc`:

    export GPG_TTY=$(tty)

    gpg-unlock() {
        gpg-connect-agent updatestartuptty /bye > /dev/null
        gpg -qd ~/.local/share/gpg-unlock.gpg > /dev/null
    }

    gpg-lock() {
        gpg-connect-agent reloadagent /bye > /dev/null
    }

`gpg-unlock` decrypts a file made once for this, which asks for the
passphrase and leaves it cached in the agent:

    echo unlock | gpg -e -r "$(cat ~/.password-store/.gpg-id)" \
        -o ~/.local/share/gpg-unlock.gpg

The agent keeps it as long as `~/.gnupg/gpg-agent.conf` says, e.g. a day:

    default-cache-ttl 86400
    max-cache-ttl 86400

[skills/withkeys](skills/withkeys/SKILL.md) tells an agent how to use it.

## Related

- [hunch]: the human, their editor and agents working together
- [memex]: a project's review threads, issues and CI results in one format

[hunch]: https://github.com/roolebo/hunch
[memex]: https://github.com/roolebo/memex
[pass]: https://www.passwordstore.org/
