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
- **Apple silicon on macOS 26** gets a prebuilt bottle and installs in seconds.
  That is the only bottle published, so every other target falls back to
  building all 39 resources from source, which pulls ~2GB of toolchain first
  (`cryptography` needs Rust, `pillow` needs six image libraries). Budget
  several minutes there.
- Intel macOS and older macOS versions are untested source builds. Linux does
  not build at all right now: `pillow` fails to compile.

Unofficial and unaffiliated with Vast.ai. Packages their MIT-licensed CLI
unmodified from the published PyPI sdist. Formula is [MIT](LICENSE).
