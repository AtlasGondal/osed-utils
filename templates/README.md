# Templates

Generic starter templates intended for OSED-style exploit development workflows.

## Contents

- [Exploit Template (Python)](#exploit-template-python)

---

## Exploit Template (Python)

`exploit-template.py` is a Python starter template for exploit development. It includes scaffolding for:

- **Shellcode placement** (`get_payload()`)
- **ROP chain layout** (examples for `VirtualProtect`, `VirtualAlloc`, and `WriteProcessMemory`)
- Optional **SEH overwrite** layout (`get_seh_overwrite()`)
- **Bad character** checking via `sanity_check()`
- Basic **socket delivery** via `get_connection()` / `send_exploit()`

**Script:** [`exploit-template.py`](exploit-template.py)

### Requirements

- Python 3
- Local helper module referenced by the template:

```bash
python exploit-template.py
```