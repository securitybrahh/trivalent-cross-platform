# Trivalent - Security-Focused Chromium Fork

<p align="center">
  <img src="https://raw.githubusercontent.com/secureblue/Trivalent/live/trivalent.svg" alt="Trivalent Logo" width="200"/>
</p>

<p align="center">
  <a href="https://github.com/secureblue/Trivalent/releases">
    <img src="https://img.shields.io/github/v/release/secureblue/Trivalent?include_prereleases&label=latest" alt="Latest Release"/>
  </a>
  <a href="https://github.com/secureblue/Trivalent/blob/live/LICENSE">
    <img src="https://img.shields.io/github/license/secureblue/Trivalent" alt="License"/>
  </a>
</p>

Trivalent is a security-focused, Chromium-based browser for desktop Linux inspired by [Vanadium](https://github.com/GrapheneOS/Vanadium). It applies privacy and security patches to Chromium to enhance user security and privacy.

## Features

- **Enhanced Security**: Built with security-hardened patches
- **Privacy-Focused**: Disables many privacy-invading features by default
- **Open Source**: Built from source with transparency
- **Regular Updates**: Automatically builds when new Chromium versions are released

## Installation

### Prerequisites

Before installing Trivalent, you may need to remove existing Chromium installations:

```bash
# Remove existing Chromium (optional)
sudo apt remove chromium chromium-browser google-chrome-stable  # Debian/Ubuntu
sudo dnf remove chromium  # Fedora/RHEL
```

### Option 1: DEB Repository (Debian/Ubuntu)

#### Automated Installation

Run the following commands:

```bash
# Replace YOUR_ORG and YOUR_REPO with your GitHub organization and repository names
REPO_URL="https://YOUR_ORG.github.io/YOUR_REPO/deb/"

echo "deb [trusted=yes] $REPO_URL ./" | sudo tee /etc/apt/sources.list.d/trivalent.list
sudo apt-get update
sudo apt-get install chromium
```

#### Manual Installation

1. Create the repository file:
```bash
sudo nano /etc/apt/sources.list.d/trivalent.list
```

2. Add the following line (replace the URL with your repository URL):
```
deb [trusted=yes] https://your-org.github.io/your-repo/deb/ ./
```

3. Update and install:
```bash
sudo apt-get update
sudo apt-get install chromium
```

### Option 2: RPM Repository (Fedora/RHEL/EL)

#### Automated Installation

Run the following commands:

```bash
# Replace YOUR_ORG and YOUR_REPO with your GitHub organization and repository names
REPO_URL="https://YOUR_ORG.github.io/YOUR_REPO/rpm/"

sudo tee /etc/yum.repos.d/trivalent.repo << EOF
[trivalent]
name=Trivalent
baseurl=$REPO_URL
enabled=1
gpgcheck=0
EOF

sudo dnf check-update
sudo dnf install chromium
```

#### Manual Installation

1. Create the repository file:
```bash
sudo nano /etc/yum.repos.d/trivalent.repo
```

2. Add the following content (replace the URL with your repository URL):
```ini
[trivalent]
name=Trivalent
baseurl=https://your-org.github.io/your-repo/rpm
enabled=1
gpgcheck=0
```

3. Install:
```bash
sudo dnf check-update
sudo dnf install chromium
```

For older RHEL/CentOS systems using yum:
```bash
sudo yum check-update
sudo yum install chromium
```

### Option 3: Install from Releases

You can also download packages directly from the GitHub Releases page:

- **Windows**: Download `chromium-windows.zip` and extract
- **Linux DEB**: Download `chromium-{version}-amd64.deb` and install with `sudo dpkg -i chromium-{version}-amd64.deb`
- **Linux RPM**: Download `chromium-{version}.rpm` and install with `sudo rpm -ivh chromium-{version}.rpm`
- **macOS**: Download `chromium-{version}-x64.dmg` and install

### Option 4: Windows Auto-Update with chrlauncher

For Windows users who want automatic updates, you can use [chrlauncher](https://github.com/richard-999/chrlauncher), a lightweight Chromium updater.

The Windows build includes an `updateurl.txt` file that chrlauncher can use for automatic updates.

#### Quick Setup

1. Download [chrlauncher](https://github.com/richard-999/chrlauncher/releases) and extract it
2. Create a `chrlauncher.ini` file with the following content:

```ini
[chrlauncher]

# Custom Chromium update URL
ChromiumUpdateUrl=https://github.com/YOUR_ORG/YOUR_REPO/releases/latest/download/updateurl.txt

# Command line for Chromium
ChromiumCommandLine=--user-data-dir="%LOCALAPPDATA%\Trivalent\User Data" --no-default-browser-check

# Chromium executable file name
ChromiumBinary=chrome.exe

# Chromium binaries directory
ChromiumDirectory=.
```

3. Run `chrlauncher.exe` - it will automatically download and update Chromium

#### Full Configuration Options

```ini
[chrlauncher]

# Custom Chromium update URL (string):
ChromiumUpdateUrl=https://github.com/YOUR_ORG/YOUR_REPO/releases/latest/download/updateurl.txt

# Command line for Chromium (string):
# note --user-data-dir= works better if path is absolute
# See here: http://peter.sh/experiments/chromium-command-line-switches/
ChromiumCommandLine=--user-data-dir="%LOCALAPPDATA%\Trivalent\User Data" --no-default-browser-check

# to enable full logging in c:\temp\log.txt (daily rotate, no automatic deletion)
# ChromiumCommandLine=--enable-logging --v=0 --log-file=C:\temp\log.txt --user-data-dir=".\User Data" --no-default-browser-check

# Chromium executable file name (string):
ChromiumBinary=chrome.exe

# Chromium binaries directory (string):
# Relative (to chrlauncher directory) or full path (env. variables supported).
ChromiumDirectory=.
```

#### How It Works

- **ChromiumUpdateUrl**: Points to the `updateurl.txt` file in your releases (auto-generated by the build)
- **ChromiumCommandLine**: Command-line arguments for Chrome (user data directory, etc.)
- **ChromiumBinary**: The executable name to run
- **ChromiumDirectory**: Where the Chrome files are stored (relative to chrlauncher)

Chrlauncher will check the URL for new versions and automatically download and extract updates.

## Building from Source

This repository contains GitHub Actions workflows that automatically build Trivalent when new Chromium versions are released.

### Build Targets

- **Windows**: `chromium-windows.zip`
- **Linux DEB**: `chromium-{version}-amd64.deb`
- **Linux RPM**: `chromium-{version}.rpm`
- **macOS**: `chromium-{version}-x64.dmg`

### Triggering a Build

1. Go to the Actions tab
2. Select "Build Chromium from Source"
3. Click "Run workflow"
4. Optionally specify a Chromium version

### Repository URLs

After enabling GitHub Pages, your repositories will be available at:
- **DEB**: `https://{org}.github.io/{repo}/deb/`
- **RPM**: `https://{org}.github.io/{repo}/rpm/`

## Security Features

Trivalent applies the following security and privacy patches:

### From Vanadium
- Disable seed-based field trials
- Disable network prediction
- Disable third-party cookies by default
- Disable background sync
- Enable strict origin isolation
- Enable partitioned cache
- Require HTTPS for component updates
- Restrict dynamic code execution
- And more...

### From Trivalent
- Force-disable Safe Browsing
- Disable sync, metrics, and variations
- Disable AI/ML features
- Enable audio service sandbox
- Set secure DNS to automatic mode
- Block external extensions
- And more...

## Configuration

### Data Directory
By default, Trivalent stores user data in `~/.config/Trivalent/`. You can change this by setting the `--user-data-dir` flag.

### Flags
Trivalent supports all Chromium flags. Access `chrome://flags` in the browser for experimental features.

## License

Trivalent is based on [Chromium](https://www.chromium.org/) and includes modifications from the [Vanadium](https://github.com/GrapheneOS/Vanadium) project. See the [LICENSE](LICENSE) file for details.

## Contributing

Contributions are welcome! Please read our contributing guidelines before submitting PRs.

## Acknowledgments

- [Chromium Project](https://www.chromium.org/)
- [GrapheneOS](https://grapheneos.org/) - For the Vanadium patches
- [secureblue](https://github.com/secureblue) - For the Trivalent patches

## Support

If you encounter issues, please file a bug report on the GitHub issue tracker.
