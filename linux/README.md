# Introduction to Linux

When a user logs into a Linux system and opens the **Files** application, they are directed to their **home directory**. Each user has a dedicated home directory within the Linux file system. This directory is located inside the system's main **root (`/`) directory**, under the `home` folder.

Within the `/home` directory, you will find individual subdirectories corresponding to each user on the system.

The **root user** is an exception. Instead of a home directory under `/home`, the root user has a separate home directory located at `/root`.

## /bin

The `bin` directory contains essential user-level executable programs and commands. These binaries are required for basic system operation and are available for all users.

## /sbin

The `sbin` directory contains system administration binaries. These commands are primarily intended for performing system-level tasks and typically require superuser (root) privileges to execute.

## /lib

/lib contains shared libraries that are required for the system’s fundamental programs to run — especially those in /bin and /sbin.

- Stores essential shared libraries needed during system boot.
- Required even before the /usr partition is mounted, which is why critical libraries must live here.

## /usr

If we look carefully usr and root has a similar directory structure. The reason / (root) and /usr have a similar directory structure (e.g., both have bin, sbin, lib) is historical and tied to how Unix systems originally booted and used storage.

When storage was limited, some of the binary files were stored /bin, /sbin and some of them were store in '/usr' folder.

One major difference is when we write the commands from terminal window, they are loaded from the `/usr` folder.

## /usr/local

/usr/local is a directory used for software installed manually by the system administrator, rather than by the operating system’s package manager. It mirrors the structure of /usr (with bin, sbin, lib, share, etc.) so that custom-installed programs can coexist cleanly with system-installed ones.

- It is designed this way so locally installed software follows the same layout as system software but stays separate.
- Easy upgrades of the OS (local files remain untouched)
- Simple removal or backup of custom tools

## /opt

The `/opt` directory is primarily used for installing **add-on or third-party application software** that is not part of the default operating system. Software placed in `/opt` is typically **self-contained**, meaning all of its components—binaries, libraries, configuration files, and documentation—are organized within a single subdirectory inside `/opt`.

This structure helps keep these applications isolated from system-managed directories such as `/usr/bin` or `/usr/lib`, reducing the risk of conflicts during system updates or package installations.

Some software vendors prefer not to divide their files across multiple system paths. Instead, they package everything into one unified directory. For such applications—common examples include IDEs and large commercial tools—`/opt` provides a clean and predictable location.

## /boot

The `/boot` directory contains all the files needed for the system to start (boot) before the main operating system loads.
