# Building Windows Binary for AI Video Editor

Since we're on macOS ARM64, the Windows binary must be built on a Windows machine with MSVC toolchain.

## Prerequisites (Windows machine)

1. **Visual Studio Build Tools**
   - Download: https://visualstudio.microsoft.com/downloads/
   - Install "Desktop development with C++"

2. **Python 3.13+**
   - Download: https://www.python.org/downloads/windows/

3. **Node.js 20+**
   - Download: https://nodejs.org/

4. **Rust**
   - Download: https://rustup.rs/

## Build Steps

```powershell
# Clone and navigate to repo
git clone https://github.com/mugilan-sakthivel/Ai-video-editor.git
cd Ai-video-editor/deep-agent

# Install Python dependencies
python -m pip install --upgrade pip
pip install -e .

# Build Python sidecar
python scripts/build_sidecar.py --target x86_64-pc-windows-msvc

# Install desktop dependencies
cd desktop
npm install

# Build Tauri app
npm run tauri:build -- --target x86_64-pc-windows-msvc
```

## Output Files

Built artifacts will be in:
- `desktop/src-tauri/target/x86_64-pc-windows-msvc/release/bundle/msi/` - Installer
- `desktop/src-tauri/target/x86_64-pc-windows-msvc/release/bundle/nsis/` - NSIS installer

## Uploading to Release Repo

Once built:

```powershell
cd /path/to/ai-video-editor-releases
git config user.email "windows-builder@example.com"
git config user.name "Windows Builder"

# Copy artifacts
mkdir -p releases/v0.1.2
cp ../Ai-video-editor/deep-agent/desktop/src-tauri/target/x86_64-pc-windows-msvc/release/bundle/msi/*.msi releases/v0.1.2/
cp ../Ai-video-editor/deep-agent/desktop/src-tauri/target/x86_64-pc-windows-msvc/release/bundle/nsis/*.exe releases/v0.1.2/

# Commit and push
git add .
git commit -m "chore: release v0.1.2 - Windows x86_64 build"
git push origin main
```

