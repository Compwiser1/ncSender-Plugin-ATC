## v0.1.63

First release from the `Compwiser1/ncSender-Plugin-ATC` repository — a continuation fork of `siganberg/ncsender-plugin-rapidchangeatc`, which is going away. Plugin behavior is unchanged and the plugin id stays `com.ncsender.rapidchangeatc`, so existing installs keep their saved settings.

### 🔧 Improvements
- Operator prompts now appear only after machine motion has fully stopped, so instructions no longer show up mid-move

### 📦 Repository
- Maintained by Compwiser1; manifest `author` and `repository` updated accordingly
- Repository layout now matches the other ncSender plugin repos (GPL-3.0 `LICENSE`, `.scripts/bump-release.sh`, tag-validating release workflow that also publishes a stable `-latest.zip`)
