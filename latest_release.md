## v0.1.64

First release from the `Compwiser1/ncSender-Plugin-ATC` repository — a continuation fork of `siganberg/ncsender-plugin-rapidchangeatc`, which is going away.

**Plugin behavior is unchanged from upstream v0.1.63.** The plugin id stays `com.ncsender.rapidchangeatc`, so an existing install keeps its saved settings when updated from this release. Only one copy of the plugin should be enabled at a time.

### 🔧 Improvements (carried from upstream v0.1.63)
- Operator prompts now appear only after machine motion has fully stopped, so instructions no longer show up mid-move

### 📦 Repository
- Maintained by Compwiser1; manifest `author` and `repository` updated accordingly
- Layout now matches the other ncSender plugin repos (GPL-3.0 `LICENSE`, `.scripts/bump-release.sh`, tag-validating release workflow that also publishes a stable `com.ncsender.rapidchangeatc-latest.zip` download URL)
