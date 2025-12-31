# Standalone Utilities

Small, self-contained scripts intended for exploit dev / debugging workflows.

## Contents

- [Egghunter](#egghunter)
- [Find Gadgets](#find-gadgets)
- [Shellcoder](#shellcoder)

---

## Egghunter

Generates an x86 egghunter compatible with the OSED lab VM. Supports:
- **NtAccessCheckAndAuditAlarm**-based hunter
- **SEH**-based hunter
- Optional **bad character** checking against the produced byte string

**Script:** [`egghunter.py`](egghunter.py)

### Install

````bash
pip3 install keystone-engine numpy
`````

### Usage

```bash
usage: egghunter.py [-h] [-t TAG] [-b BAD_CHARS [BAD_CHARS ...]] [-s]

Creates an egghunter compatible with the OSED lab VM

optional arguments:
  -h, --help            show this help message and exit
  -t TAG, --tag TAG     tag for which the egghunter will search (default: c0d3)
  -b BAD_CHARS [BAD_CHARS ...], --bad-chars BAD_CHARS [BAD_CHARS ...]
                        space separated list of bad chars to check for in final egghunter (default: 00)
  -s, --seh             create an seh based egghunter instead of NtAccessCheckAndAuditAlarm
```

#### Examples
```bash
./egghunter.py                              # generate default egghunter
./egghunter.py --tag w00t                   # generate egghunter with w00tw00t tag
./egghunter.py -b 00 0a 25 26 3d --seh      # generate SEH-based egghunter while checking for bad characters (does not alter the shellcode, that's to be done manually)
`````

## Find Gadgets
Finds and categorizes useful gadgets. Only prints to terminal the cleanest gadgets available (minimal amount of garbage between what's searched for and the final ret instruction). All gadgets are written to a text file for further searching.

### Install

```bash
pip3 install rich ropper
```

**Script:** [`find-gadgets.py`](find-gadgets.py)

### Usage

```bash
usage: find-gadgets.py [-h] -f FILES [FILES ...] [-b BAD_CHARS [BAD_CHARS ...]] [-o OUTPUT]

Searches for clean, categorized gadgets from a given list of files

optional arguments:
  -h, --help            show this help message and exit
  -f FILES [FILES ...], --files FILES [FILES ...]
                        space separated list of files from which to pull gadgets (optionally, add base address (libspp.dll:0x10000000))
  -b BAD_CHARS [BAD_CHARS ...], --bad-chars BAD_CHARS [BAD_CHARS ...]
                        space separated list of bad chars to omit from gadgets, e.g., 00 0a (default: empty)
  -o OUTPUT, --output OUTPUT
                        name of output file where all (uncategorized) gadgets are written (default: found-gadgets.txt)
```

#### Examples
```bash
./find-gadgets.py -f libeay32ibm019.dll     # search gadgets in one file
./find-gadgets.py -f libeay32ibm019.dll -s  # skip rp++ enrichment (only use ropper)
./find-gadgets.py -f a.dll b.dll -b 00 0a   # search gadgets in multiple files
./find-gadgets.py -f a.dll b.dll -b 00 0a   # search gadgets in multiple files
./find-gadgets.py -f libspp.dll:0x10000000  # specify an image base for a module
````

## Shellcoder
Creates reverse shell with optional msi loader

```bash
usage: shellcoder.py [-h] [-l LHOST] [-p LPORT] [-b BAD_CHARS [BAD_CHARS ...]] [-m] [-d] [-t] [-s]

Creates shellcodes compatible with the OSED lab VM

**Script:** [`shellcoder.py`](shellcoder.py)

### Usage
optional arguments:
  -h, --help            show this help message and exit
  -l LHOST, --lhost LHOST
                        listening attacker system (default: 127.0.0.1)
  -p LPORT, --lport LPORT
                        listening port of the attacker system (default: 4444)
  -b BAD_CHARS [BAD_CHARS ...], --bad-chars BAD_CHARS [BAD_CHARS ...]
                        space separated list of bad chars to check for in final egghunter (default: 00)
  -m, --msi             use an msf msi exploit stager (short)
  -d, --debug-break     add a software breakpoint as the first shellcode instruction
  -t, --test-shellcode  test the shellcode on the system
  -s, --store-shellcode
                        store the shellcode in binary format in the file shellcode.bin
```

#### Examples
```bash
python3 shellcoder.py                            # generate a reverse shell payload (defaults: 127.0.0.1:4444)
python3 shellcoder.py -l 127.0.0.1 -p 3333       # generate a reverse shell for specific listener
python3 shellcoder.py --msi -l 127.0.0.1 -s      # generate MSI stager and store raw bytes
python3 shellcoder.py -t                        # test-run the shellcode (only works when run from 32-bit Python environment)
```
