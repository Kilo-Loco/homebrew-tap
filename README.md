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
links the binary into `bin`, so Homebrew owns the install and `brew upgrade`
owns the upgrade.

This tap is unofficial and is not affiliated with Vast.ai. It packages their
MIT-licensed CLI unmodified from the published PyPI sdist. Vast.ai is welcome to
adopt, fork or replace it at any time.

## What `vastai update` does here

Nothing, by design. The CLI detects the managed (`curl | bash`) install
structurally, so on a Homebrew install it declines and exits 1:

```
This CLI was not installed with the managed installer, so it cannot self-update.
  Installed via pip? Run: pip install --upgrade vastai
  Or switch to the managed install: curl -fsSL https://vast.ai/install.sh | bash
```

Declining is the correct behaviour and is what lets this formula exist at all.
Note that neither suggestion in that message applies to a Homebrew install:
upgrade with `brew upgrade vastai` instead.

## If you already ran `install.sh`

The managed installer puts `vastai` in `~/.local/bin`, which usually sits ahead
of `/opt/homebrew/bin` in `PATH` and will shadow the Homebrew copy. Remove it
first:

```bash
vastai uninstall
```

Check which one you are running with `which -a vastai`.

## Verify

```bash
brew install --build-from-source Kilo-Loco/tap/vastai
brew test Kilo-Loco/tap/vastai
brew audit --strict Kilo-Loco/tap/vastai
brew style Kilo-Loco/tap/vastai
```

Everything builds from source: 39 checksummed resources pinned to the v1.5.5
sdist. Two of them set the cost. `cryptography` needs a Rust toolchain (which
pulls LLVM) and `pillow` needs six image libraries, so a first-time install
downloads roughly 2GB of build dependencies before compiling anything. With the
toolchain already present the build takes a little over two minutes on Apple
silicon.

Verified on macOS arm64 (Tahoe, Apple silicon). Not yet verified on Intel macOS
or on Linux.

## License

The formula in this repository is [MIT licensed](LICENSE).

The packaged software is separate: `vast-cli` is MIT licensed by Vast.ai and is
installed unmodified from their published PyPI sdist. This tap claims no
ownership of it.
