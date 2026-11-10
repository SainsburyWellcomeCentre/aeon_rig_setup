# aeon_rig_setup

Setup and health checks for Project Aeon rig machines.

This repository takes a machine from a fresh Windows installation to a rig that
can be trusted, and it proves the result. It covers the BIOS, Windows, the
devices and the verification. It does not hold experiment code.

## Why it exists

Between July and October 2026 two rigs crashed repeatedly for three months. The
cause was the processor. AEON4 and AEON3 both ran Intel 13th and 14th Generation
parts that had never received Intel's microcode fix for the Vmin Shift
Instability defect.

Two things became clear during that work.

**Rig knowledge was undocumented.** Which BIOS and Windows settings matter, why
each one is set, and what happens when one is wrong.

**A setting that nobody verifies will drift.** AEON3 and AEON4 were bought
together and built the same way, yet they ran different BIOS versions, different
microcode and different memory. Nobody knew until somebody measured.

## Layout

```
checks/     read only. No administrator rights. Safe on a running rig
setup/      scripts that change the machine
docs/       one document for each stage
rigs/       one page for each machine, recording what it has
```

## The order of the stages

The order matters. Each position has a reason.

| Stage | Name | Why it is in this position |
|---|---|---|
| 0 | Identify the machine | A BIOS flash erases every setting made after it |
| 1 | BIOS | Microcode, power limits, memory, boot order, power behaviour |
| 2 | Windows installation | Edition, partitions, and what to leave out |
| 3 | Windows settings | Updates, power, scheduling, crash dumps, page file, time |
| 4 | Remote access | Console access, Wake on LAN, firewall |
| 5 | Devices | Harp serial ports, USB power, cameras, antivirus |
| 6 | Bonsai | Priority, crash dumps, packages without internet |
| 7 | Verify | One script that reads every setting and reports the wrong ones |
| 8 | Record | A page for the machine in `rigs/` |

**Stage 7 keeps the rest honest.** A runbook that nobody can check against will
drift, which is how two identical machines became different.

## Start here

Run the two checks on any machine. They need no administrator rights and change
nothing.

```powershell
powershell -NoProfile -ExecutionPolicy Bypass -File ".\checks\CheckCpuMicrocode.ps1"
powershell -NoProfile -ExecutionPolicy Bypass -File ".\checks\CheckCrashHistory.ps1"
```

Both also run against another machine:

```powershell
Invoke-Command -ComputerName RIG2 -FilePath ".\checks\CheckCpuMicrocode.ps1"
```

## How to contribute

`main` is protected. Make a branch, open a pull request, and merge it after
review.

## Licence

BSD 3-Clause. See [LICENSE](LICENSE).
