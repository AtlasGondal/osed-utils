# OSED-Utils

A small collection of utilities and starter templates intended for OSED-style exploit development and Windows debugging workflows.

## Repository Structure

- **[WinDbg helpers](windbg/README.md)**  
  WinDbg-related helpers, workspaces, and supporting tooling.

- **[PowerShell helpers](powershell/README.md)**  
  PowerShell utilities for common debugging automation (attach, restart targets, etc.).

- **[Standalone utilities](standalone/README.md)**  
  Self-contained scripts for common exploit-dev tasks (e.g., egghunters, gadget discovery, shellcode generation).

- **[Templates](templates/README.md)**  
  Starter templates for building exploits and repeatable lab workflows.

## Quick Index

### Standalone
See: **[`standalone/README.md`](standalone/README.md)**  
Includes utilities such as:
- `egghunter.py` — x86 egghunter generator (NtAccessCheckAndAuditAlarm / SEH variants)
- `find-gadgets.py` — categorized gadget finder / ROP helper output
- `shellcoder.py` — x86 shellcode generator (reverse shell, stagers, testing options)

### PowerShell
See: **[`powershell/README.md`](powershell/README.md)**  
Includes utilities such as:
- `attach-process.ps1` — automate starting/restarting targets and attaching WinDbg with optional initial commands

### WinDbg
See: **[`windbg/README.md`](windbg/README.md)**  
Includes:
- WinDbg configuration and usage helpers intended to speed up debugging loops

### Templates
See: **[`templates/README.md`](templates/README.md)**  
Includes:
- `exploit-template.py` — Python exploit skeleton with shellcode + ROP/SEH scaffolding and bad-char checks

## License

MIT. See [`LICENSE`](LICENSE)