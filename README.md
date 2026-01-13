# NTMemory - Usermode NT Explorer

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Windows%2010%2F11-blue?style=for-the-badge&logo=windows" alt="Platform">
  <img src="https://img.shields.io/badge/Mode-100%25%20Usermode-green?style=for-the-badge" alt="Usermode">
  <img src="https://img.shields.io/badge/Language-C%2FC%2B%2B-orange?style=for-the-badge&logo=c%2B%2B" alt="Language">
  <img src="https://img.shields.io/badge/UI-Dear%20ImGui-purple?style=for-the-badge" alt="ImGui">
</p>

---

## Overview

**NTMemory** is a Windows system analysis tool that provides deep insight into kernel internals, memory management, and system structures entirely from usermode. This application does not load any kernel drivers. All information is retrieved using documented and undocumented NT APIs available from usermode, including NtQuerySystemInformation and the Superfetch interface.

---

## Features

### Memory Analysis
- **Physical Memory Usage** — Real-time monitoring of total and available physical RAM
- **Page File Statistics** — Pagefile usage, peak usage, and total capacity
- **Commit Charge** — Committed pages, commit limit, and peak commitment tracking
- **File Cache Statistics** — Current and peak file cache sizes with page fault counts
- **Memory History Graphs** — Visual graphs tracking standby and free memory over time
- **8-Priority Standby List Breakdown** — Visualize pages across all 8 standby priority levels (P0-P7) with color-coded bar charts
- **Memory List Tracking** — Monitor zeroed pages, free pages, modified pages, modified no-write, and bad pages
- **Pool Statistics** — Kernel paged and non-paged pool usage and allocation counts
- **Memory Compression (Windows 10+)** — Compression process identification, store working set, compression statistics, ratio calculation, and memory savings

### Kernel Inspection
- **Process and Thread Enumeration** — EPROCESS and KTHREAD kernel addresses for every process and thread
- **Session and Parent Information** — Session ID and parent process relationships with expandable tree view
- **Driver Enumeration** — Complete list of loaded kernel-mode drivers with load order, base addresses, image sizes, and full paths
- **Kernel Module Information** — ntoskrnl.exe base address, size, and filename (highlighted in driver list)
- **Module Export Analysis** — Complete export tables for any kernel module including ntoskrnl.exe, hal.dll, win32k.sys, ntfs.sys, and other system modules
- **Export Details** — Function names with syntax highlighting, ordinals, RVA values, calculated kernel addresses, and forwarder detection
- **Current Process Information** — Self EPROCESS and KTHREAD addresses for reference

### Physical Memory and PFN Database
- **Physical Memory Mapping** — All physical memory ranges retrieved from Superfetch with start PFN, page count, and size display
- **Virtual to Physical Translation** — Convert virtual addresses to physical addresses with PFN lookup
- **PFN Database Queries** — Query any range of PFNs with use description, list description, priority levels, pinned/image flags, and virtual address mapping
- **Query Performance Metrics** — Timing information for PFN database queries

### Process Memory Analysis
- **Per-Process Statistics** — Working set, peak working set, private bytes, pagefile usage, and page fault counts
- **Superfetch Integration** — Extended process data including shareable working set, shared working set, and private working set calculations
- **Process Filtering** — Search and filter processes by name

### System Monitoring
- **Pool Tag Analysis** — Complete list of kernel pool allocations with tag names, paged/non-paged pool usage, allocation counts, and sortable columns (auto-refresh every 5 seconds)
- **Handle Statistics** — System-wide handle count, unique processes with handles, handle type breakdown, and sorted display by frequency
- **Performance Metrics** — I/O operations (read/write/other) with byte counts, per-processor kernel time, user time, DPC time, and interrupt counts
- **Context Switches and System Calls** — System-wide counters

### Prefetch Analysis
- **Application Launch History** — Programs that have been launched with file sizes, last access times, sorted by most recent

---

## Technical Implementation

### NT APIs Utilized (All Usermode)

| API | Purpose |
|-----|---------|
| `NtQuerySystemInformation` | System information queries |
| `SystemModuleInformation` (Class 11) | Kernel driver enumeration |
| `SystemHandleInformation` | Handle table queries |
| `SystemExtendedHandleInformation` | Extended handle data |
| `SystemPerformanceInformation` | Performance counters |
| `SystemProcessorPerformanceInformation` | Per-CPU statistics |
| `SystemPoolTagInformation` | Pool tag enumeration |
| `SystemFileCacheInformation` | File cache statistics |
| `SystemMemoryListInformation` | Memory list details |
| `SystemSuperfetchInformation` | Superfetch/SysMain queries |
| `SystemStoreInformation` | Memory compression data |

### Superfetch Information Classes
- `SuperfetchMemoryRangesQuery` — Physical memory ranges
- `SuperfetchPfnQuery` — PFN database queries
- `SuperfetchPrivSourceQuery` — Process working set data

### PE Parsing
Manual PE header parsing for kernel module exports with export directory enumeration, forwarder detection, and RVA to kernel address calculation.

---

## Requirements

- **Operating System:** Windows 10/11 (64-bit)
- **Privileges:** Administrator privileges required for NT API access
- **Services:** SysMain service should be running for Superfetch features
- **Runtime:** Visual C++ Redistributable 2019 or later

### Automatically Enabled Privileges
- `SeDebugPrivilege` — Process and thread access
- `SeProfileSingleProcessPrivilege` — Performance data access
- `SeSystemProfilePrivilege` — System profiling access

---

## Building

### Requirements
- Visual Studio 2019 or later
- Windows SDK 10.0.19041.0 or later
- DirectX 11 SDK (included in Windows SDK)

### Build Steps
```bash
# Clone the repository
git clone https://github.com/yourusername/NTMemory.git

# Open in Visual Studio
# Build -> Build Solution (Release x64)
```

### Dependencies
- **Dear ImGui** — Immediate mode GUI library (included)
- **DirectX 11** — Hardware-accelerated rendering
- **ntdll.lib** — NT Native API import library

---

## Disclaimer

This tool is intended for **educational and research purposes only**. It provides insight into Windows internals that may be useful for system administrators, security researchers, malware analysts, Windows internals enthusiasts, and driver developers.

All information is retrieved through legitimate usermode APIs. No kernel code is executed. This application does not function as an exploitation or hacking tool.

---

## License

This project is provided as-is for educational purposes. See LICENSE file for details.

---

## Acknowledgments

- **Dear ImGui** by Omar Cornut — Immediate mode GUI library
- **Windows Internals** by Yosifovich, Ionescu, Russinovich, Solomon — Essential reference material
- **ReactOS** — Open-source Windows-compatible OS providing structure references
- **Geoff Chappell** — Comprehensive undocumented Windows documentation

---
