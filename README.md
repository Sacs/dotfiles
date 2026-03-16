# dotfiles
This is just my collection of dotfiles that I use on mac, linux and wsl2

Feel free to use what you like...

## Git config structure

The main [.gitconfig](.gitconfig) uses `include` and `includeIf` to split configuration across several files:

| File | Tracked | Loaded when |
| --- | --- | --- |
| `~/.gitconfig-user` | No | Always |
| `~/.gitconfig-home-local` | No | Inside `~/projects/personal/` |
| `~/.gitconfig-home` | Yes | Inside `~/projects/personal/` |
| `~/.gitconfig-work-local` | No | Inside `~/projects/work/` |
| `~/.gitconfig-work` | Yes | Inside `~/projects/work/` |

The `-local` files and `gitconfig-user` are **not** committed to this repo because they contain machine- or identity-specific values. You must create them yourself.

### `~/.gitconfig-user` (required)

Contains your personal identity. Always loaded regardless of which project you are in.

```ini
[user]
  name = Your Name
  email = you@example.com
  signingKey = ABC1234DEF5678  # optional, only needed if you sign commits
```

### `~/.gitconfig-home-local` (optional)

Local overrides applied only to repos under `~/projects/personal/`. Useful for overriding your identity for personal work without affecting work repos.

```ini
[user]
  email = personal@example.com  # override if different from gitconfig-user
```

If you only have one identity or don't need personal-project-specific overrides, this file can be empty or omitted entirely.

### `~/.gitconfig-work-local` (required if using `.gitconfig-work`)

Local overrides applied only to repos under `~/projects/work/`. The `delete-merged-branches` alias in [.gitconfig-work](.gitconfig-work) reads `branch.protected` from config to know which branches to never delete — this is where you set that list.

```ini
[user]
  email = you@yourcompany.com
  name = Your Name  # optional, only if it differs from gitconfig-user

[branch]
  protected = master main dev stage  # space-separated list of branches to protect
```

The `branch.protected` value is **required** for the `delete-merged-branches` alias to work correctly. If it is not set, the alias falls back to `master main dev`.
