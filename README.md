# homebrew-tap

Homebrew formula for the [Vast.ai CLI](https://github.com/vast-ai/vast-cli).

```bash
brew install Kilo-Loco/tap/vastai
```

Use the fully-qualified name. Homebrew trusts a third-party formula you name
explicitly, so that one command is all you need.

Tapping first and installing by the bare name does *not* work on its own:

```bash
brew tap Kilo-Loco/tap
brew install vastai
# Error: Refusing to load formula kilo-loco/tap/vastai from untrusted tap
```

If you prefer that form, trust the tap once first:

```bash
brew trust kilo-loco/tap
```

## Why this exists

`vast-cli` ships a `curl | bash` installer and a PyPI package, but no Homebrew
path. This tap provides one.

The formula installs `vastai` into an isolated virtualenv under `libexec` and
links the `vastai` binary into `bin`, so Homebrew owns the install and
`brew upgrade` owns the upgrade. The CLI's own `vastai update` is a no-op here:
it detects non-managed installs structurally and refuses to touch them.

This tap is unofficial. It tracks upstream releases and is offered to Vast.ai to
adopt or replace at any time — see
[the upstream discussion](https://github.com/vast-ai/vast-cli/issues).

## If you already ran `install.sh`

The managed installer puts `vastai` in `~/.local/bin`, which usually sits ahead
of `/opt/homebrew/bin` in `PATH` and will shadow the Homebrew copy. Remove it
first:

```bash
vastai uninstall
```

## Verify

```bash
brew test Kilo-Loco/tap/vastai
brew audit --strict Kilo-Loco/tap/vastai
```
