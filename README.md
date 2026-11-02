# alacritty config

<p>
  <a href="./LICENSE">
    <img
      alt="License"
      src="https://img.shields.io/github/license/acfatah/alacritty-config?style=flat-square"
    ></a>
  <a href="https://github.com/acfatah/alacritty-config/commits/main">
    <img
      alt="Last commit"
      src="https://img.shields.io/github/last-commit/acfatah/alacritty-config?display_timestamp=committer&style=flat-square"
    ></a>
</p>

On windows platform, checkout this repository to
`C:\Users\<USER>\AppData\Roaming\alacritty`. The commands below resolve that
path for you, so they can be copied as is.

From cmd:

```bat
git clone git@github.com:acfatah/alacritty-config.git "%APPDATA%\alacritty"
```

From PowerShell:

```powershell
git clone git@github.com:acfatah/alacritty-config.git "$env:APPDATA\alacritty"
```

or using wsl, which asks Windows for `%APPDATA%` and converts it with
`wslpath`:

```bash
APPDATA_WSL="$(wslpath "$(cmd.exe /c echo %APPDATA% 2>/dev/null | tr -d '\r')")"
git clone git@github.com:acfatah/alacritty-config.git "$APPDATA_WSL/alacritty"
```

## Config files

| File             | Launches                                              |
| ---------------- | ----------------------------------------------------- |
| `alacritty.toml` | Default config, loaded automatically (PowerShell)     |
| `ps.toml`        | PowerShell, Alacritty's default shell on Windows      |
| `wsl.toml`       | WSL (`wsl.exe --cd ~`), starting in the Linux home    |
| `gitbash.toml`   | Git Bash as a login shell, from Git for Windows       |

`wsl.toml` imports its color theme from
`themes/github_dark_high_contrast.toml`. The others import
`themes/xterm.toml`.

## Starting Alacritty with a config

Starting `alacritty.exe` with no arguments loads `alacritty.toml` from this
folder automatically. To use another config, pass its path with
`--config-file`.

The examples below assume the default installer path. See
[Tips](#tips) if yours differs.

### Option 1: Windows shortcut

1. Right-click the desktop and choose **New → Shortcut**.
2. Enter this as the location (target):

   ```
   "C:\Program Files\Alacritty\alacritty.exe" --config-file "%APPDATA%\alacritty\wsl.toml"
   ```

3. Name it, e.g. `Alacritty WSL`, and click **Finish**.
4. Repeat with `ps.toml` or `gitbash.toml` for a PowerShell or Git Bash
   shortcut, if wanted.

Optional tweaks in the shortcut's **Properties**:

- **Start in**: set to `%USERPROFILE%` to open in your home folder. Git Bash
  starts in this folder, so set it for a Git Bash shortcut.
- **Shortcut key**: assign a global hotkey, e.g. `Ctrl + Alt + T`.
- **Change Icon…**: browse to `alacritty.exe` to use its icon.

To launch it from the taskbar or Start menu:

- Right-click the shortcut and choose **Pin to taskbar** or **Pin to Start**.
- Or move it into the Start menu folder, so Windows search finds it by name:

  ```
  %APPDATA%\Microsoft\Windows\Start Menu\Programs
  ```

### Option 2: Run dialog

Press `Win + R`, paste the command, and press Enter:

```
"C:\Program Files\Alacritty\alacritty.exe" --config-file "%APPDATA%\alacritty\wsl.toml"
```

### Option 3: PowerShell or cmd

Run it directly from PowerShell:

```powershell
alacritty --config-file "$env:APPDATA\alacritty\wsl.toml"
```

Or from cmd:

```bat
alacritty --config-file "%APPDATA%\alacritty\wsl.toml"
```

To make it a short command, add a function to your PowerShell profile
(open it with `notepad $PROFILE`):

```powershell
function alawsl { alacritty --config-file "$env:APPDATA\alacritty\wsl.toml" }
function alaps  { alacritty --config-file "$env:APPDATA\alacritty\ps.toml" }
function alagit { alacritty --config-file "$env:APPDATA\alacritty\gitbash.toml" }
```

### Option 4: From inside WSL

Call the Windows executable with the Windows config path. `cmd.exe` supplies
`%APPDATA%`, and `tr` strips the carriage return it prints:

```bash
alacritty.exe --config-file "$(cmd.exe /c echo %APPDATA% 2>/dev/null | tr -d '\r')\alacritty\wsl.toml"
```

To make it a short command, add an alias to `~/.bashrc` (or `~/.zshrc`):

```bash
alias alawsl='alacritty.exe --config-file "$(cmd.exe /c echo %APPDATA% 2>/dev/null | tr -d "\r")\alacritty\wsl.toml"'
```

If `alacritty.exe` is not found, use the full path:
`/mnt/c/Program\ Files/Alacritty/alacritty.exe`.

### Option 5: Make one config the default

If you always want the same config, make `alacritty.toml` use it. Every
plain launch (Start menu, taskbar, `alacritty`) then picks it up. Either
copy the content of e.g. `wsl.toml` into `alacritty.toml`, or import it:

```toml
[general]
import = ["wsl.toml"]
```

## Tips

- **Install path differs** for winget, Scoop, or portable installs. Find
  yours with `where.exe alacritty` in cmd, or
  `(Get-Command alacritty).Source` in PowerShell.
- **Git installed elsewhere?** Edit `program` in `gitbash.toml`. Find Git
  with `where.exe git`; `bash.exe` sits in the `bin` folder next to `cmd`.
- **One-off overrides** don't need a new file. Use `-o` / `--option`,
  e.g. `alacritty --config-file "%APPDATA%\alacritty\wsl.toml" -o font.size=11`.
- **Live reload**: saving a config file applies the change to open windows,
  so there is no need to restart.
- **Use absolute paths** (or `%APPDATA%`) in shortcuts. A relative
  `--config-file` path resolves against the shortcut's *Start in* folder.

## License

This repository is released under the MIT license; see [LICENSE](./LICENSE).

The theme files in `themes/` come from
https://github.com/alacritty/alacritty-theme and are licensed under the
Apache-2.0 license.
