# KernelTool

**KernelTool** is a versatile shell script that automates various tasks related to kernel debugging, filesystem extraction, file archiving, and core dump configuration. It is designed to assist developers and system administrators in managing kernel debugging sessions, extracting compressed filesystems, and setting up root filesystems for testing.

## Features

- **Filesystem Extraction**: Extracts compressed files (e.g., gzip) into a `rootfs` directory.
- **Root Filesystem Archiving**: Creates `.cpio` or `.cpio.gz` archives with custom binaries for testing.
- **Kernel Debugging Setup**: Configures GDB for kernel debugging with pre-set breakpoints.
- **Kernel Image Extraction**: Extracts a compressed kernel image (`bzImage`) into a `vmlinux` file.
- **Core Dump Configuration**: Configures core dump settings and prepares the environment for kernel crash analysis.

## Requirements

- `gcc` - for compiling `exp.c` if you use the `-mer` options.
- `gdb` - for kernel debugging when using the `-gdb` option.
- `cpio`, `gzip`, `gunzip` - for filesystem extraction and archiving.
- `wget` - for downloading `extract-vmlinux` if it is not already available.

## Usage

```bash
./KernelTool.sh <option> [file_name]
```

### Options

- `-ext <file_name>`: Extracts the specified compressed file (e.g., gzip) into the `rootfs` directory.
- `-ext -vmlinux`: Extracts `bzImage` to `vmlinux` using the `extract-vmlinux` script.
- `-mer -cpio`: Compiles `exp.c`, adds it to `rootfs`, and creates a `rootfs.cpio` archive.
- `-mer -gz -cpio`: Compiles `exp.c` into a static binary, adds it to `rootfs`, and creates a compressed archive `rootfs.cpio.gz`.
- `-gdb`: Configures GDB for kernel debugging, sets the architecture, and connects to a remote session on `localhost:1234`.

### Examples

```bash
./KernelTool.sh -ext file.cpio.gz
./KernelTool.sh -ext -vmlinux
./KernelTool.sh -mer -cpio
./KernelTool.sh -mer -cpio -gz 
./KernelTool.sh -gdb
```

## Functionality Overview

1. **Filesystem Extraction (`-ext <file_name>`)**  
   - Deletes and recreates `rootfs` directory.
   - Copies the specified file to `rootfs` and decompresses if it’s gzip-compressed.
   - Extracts the archive using `cpio`.

2. **Kernel Image Extraction (`-ext -vmlinux`)**  
   - Downloads the `extract-vmlinux` script if not available.
   - Extracts `bzImage` to produce a `vmlinux` file for debugging.

3. **Root Filesystem Archiving (`-mer -cpio` and `-mer -cpio -gz`)**  
   - Compiles `exp.c` into a static binary.
   - Moves the compiled binary to `rootfs`.
   - Creates a `.cpio` or `.cpio.gz` archive for the `rootfs` directory.

4. **Kernel Debugging Setup (`-gdb`)**  
   - Sets up GDB for kernel debugging.
   - Configures the architecture and attaches to a remote session.
   - Sets breakpoints and continues execution.


