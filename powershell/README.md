# Attach Process (WinDbg)

`attach-process.ps1` is a WinDbg attach helper intended for quick OSED-style debugging loops. It can optionally **start a target executable** or **(re)start a Windows service**, then attach WinDbg to the target process and run optional initial debugger commands.

**Script:** [`attach-process.ps1`](attach-process.ps1)

---

## Requirements

- WinDbg installed (Windows Kits Debuggers)
- PowerShell

> If your system blocks script execution, you may need:
>
> ```powershell
> Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypass
> ````

---

## Usage

```powershell
[attach-process.ps1](http://_vscodecontentref_/0) [-commands <string>] [-service-name <string>] [-process-name <string>] [-path <string>]
```

#### Examples
```powershell
# attach to a service-backed process and run WinDbg commands on attach
.\attach-process.ps1 -service-name fastbackserver -process-name fastbackserver -commands '.load pykd; bp fastbackserver!recvfrom'

# attach to a service (no extra commands)
.\attach-process.ps1 -service-name 'Sync Breeze Enterprise' -process-name syncbrs

# start an EXE by path then attach
.\attach-process.ps1 -path C:\Windows\System32\notepad.exe -process-name notepad

# run in a loop (useful when repeatedly crashing a service)
while ($true) {
  .\attach-process.ps1 `
    -process-name PROCESS_NAME `
    -commands '.load pykd; bp SOME_ADDRESS; g; !exchain'
}

# load pykd, set breakpoint (let's assume a pop-pop-ret gadget) and then resume execution. Once it hits the first access violation, it will run !exchain and then g to allow execution to proceed until it hits PPR gadget, after which it steps thrice using p, bringing EIP to the instruction directly following the pop-pop-ret
while ($true) {\\tsclient\shared\osed-scripts\attach-process.ps1 -process-name PROCESS_NAME -commands '.load pykd; bp PPR_ADDRESS; g; !exchain; g; p; p; p;' ;}
```