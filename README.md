# homebrew-tap

Homebrew formula for the [Vast.ai CLI](https://github.com/vast-ai/vast-cli),
which ships a `curl | bash` installer and a PyPI package but no Homebrew path.

```bash
brew install Kilo-Loco/tap/vastai
```

Use the full name. `brew tap` plus `brew install vastai` fails with
`Refusing to load formula ... from untrusted tap` unless you first run
`brew trust kilo-loco/tap`.

### Notes

- Upgrade with `brew upgrade vastai`. The CLI's own `vastai update` correctly
  declines on a Homebrew install, but suggests pip and the `curl | bash`
  installer, neither of which applies here.
- If you previously ran `install.sh`, its `~/.local/bin/vastai` will shadow this
  one. Run `vastai uninstall` first, or check with `which -a vastai`.
- **Apple silicon on macOS 26 or 15** gets a prebuilt bottle and installs in
  seconds. Anything else builds all 39 resources from source, which pulls ~2GB
  of toolchain first (`cryptography` needs Rust, `pillow` needs six image
  libraries). Budget several minutes there.
- Intel macOS cannot be bottled: homebrew-core no longer ships Intel bottles
  for part of the dependency chain, so it is an untested source build.
- Linux is unsupported (`depends_on :macos`): `pillow`'s setup.py needs a
  `*_ROOT` hint per native library there, and after two fix rounds (zlib, then
  jpeg) the chase was stopped. Linux users should use `pip install vastai` or
  the upstream installer.

Unofficial and unaffiliated with Vast.ai. Packages their MIT-licensed CLI
unmodified from the published PyPI sdist. Formula is [MIT](LICENSE).
