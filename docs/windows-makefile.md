# Running Makefiles in Windows

> Created: May 27, 2025

To use Makefiles in Windows, you'll need to install and configure GNU Make. Here's how to set it up:

## Installation Steps

1. Install GNU Make using winget:
```powershell
winget install -e --id GnuWin32.Make
```

2. Add GNU Make to System Path:
   - Open PowerShell as Administrator
   - Run the following command to add GNU Make to your user's environment path:
```powershell
$userPath = [Environment]::GetEnvironmentVariable("Path", "User")
$newPath = "C:\Program Files (x86)\GnuWin32\bin"
if ($userPath -notlike "*$newPath*") {
    [Environment]::SetEnvironmentVariable("Path", "$userPath;$newPath", "User")
}
```

3. Verify Installation:
   - Open a new PowerShell window
   - Run the following command:
```powershell
make --version
```

## Usage

After installation, you can use `make` commands in PowerShell or Command Prompt just like on Unix-based systems:

```powershell
make            # Run default target
make <target>   # Run specific target
```

## Common Issues

1. If you get "make is not recognized as an internal or external command":
   - Make sure you've added the correct path to your environment variables
   - Try reopening your terminal/PowerShell window
   - Verify the installation path matches the path you added to environment variables

2. Line Ending Issues:
   - Windows uses CRLF (`\r\n`) while Unix uses LF (`\n`) for line endings
   - Configure Git to handle line endings properly:
```powershell
git config --global core.autocrlf true
```
   - When `core.autocrlf` is set to `true`:
     - Git will convert LF to CRLF when checking out code on Windows
     - Git will convert CRLF to LF when committing code
     - This ensures compatibility between Windows and Unix systems
     - Particularly important for Makefiles as they require LF line endings to work properly 