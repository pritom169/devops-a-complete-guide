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

> All the folders we have seen until now are read only folders

## /etc

The /etc directory contains system-wide configuration files for Linux.
If a program, service, or part of the operating system needs configuration, it almost always reads it from /etc.

## /dev

The /dev directory contains device files, which act as interfaces to hardware devices or virtual devices on the system.

- It exposes hardware (like disks, USB drives, keyboards) as files so Linux can interact with them using standard file operations.
- Programs can read from or write to these device files as if they were normal files.

## /var

The /var directory contains variable data — files that change frequently as the system runs.

## /tmp

The /tmp directory is used for storing temporary files created by applications, scripts, and the operating system.

- Holds short-lived, temporary data
- Used by programs during installation, updates, caching, or when storing intermediate data
- Safe to delete (the system can recreate needed files)

## /media

The /media directory is used for automatically mounted removable devices, such as USB drives, external hard disks, SD cards, and DVDs. When you plug in a removable device, modern Linux systems automatically create a subdirectory under /media/<username>/ and mount the device there. This provides a clean, predictable location for users to access external storage without needing manual mount commands.

## /mnt

The /mnt directory is intended for temporary, manual mounts performed by system administrators. It serves as a generic mount point for tasks such as mounting additional disks, network file systems, ISO images, or partitions during system maintenance or troubleshooting. Unlike /media, nothing is auto-mounted here—administrators explicitly use /mnt when they need a simple, temporary place to mount a filesystem.

#### Key Points

Many of these directories operate automatically behind the scenes during regular system use. The user does not necessarily interact with

- Removable devices, such as USB drives, are automatically mounted under `/media`.
- System configuration updates automatically adjust the relevant files within `/etc`.
- When software is installed, the package manager places all required files into their appropriate system directories (e.g., `/bin`, `/lib`).
- When a software needs to be uninstalled, the package manager deletes all the necessary files from those designated folders.

## Hidden Files

Hidden files in Linux are used to store configuration data and other important information that should not be modified or deleted accidentally. These files are typically created by applications or the operating system to maintain settings and environment preferences.

- Hidden files begin with a leading dot (`.`).
- They are commonly referred to as **dotfiles** in UNIX-based systems.
- They help ensure critical configuration data remains unobtrusive and protected from casual modification.

# Software Package

A software package is a compressed archive that contains all the files required for an application to run, including executables, libraries, configuration files, and metadata. Many applications also rely on additional components, known as dependencies, which must be installed for the software to function correctly. For example, installing Firefox may require several supporting libraries that must be present on the system.

Unlike Windows, where a program’s files typically reside in a single directory, Linux distributes application files across multiple system locations such as `/bin`, `/lib`, and others. Because these files are spread throughout the file system, manually installing or removing software is impractical and error‑prone. This is why Linux relies on package managers, which handle installation, dependency resolution, file placement, and clean removal in a controlled and consistent manner.

# Package Manager

A package manager downloads all the necessary packages from the internet. It also ensures the integrity and authenticity of those packages. As a software installation has dependencies, it will go out and also install those dependencies as well. Even if the dependencies has other dependencies, the package manager will install them too.

As in Linux files are distributed into different directories, it will put all the necessary files into those directories too. When it comes to upgrading the software, it will search only the files requires to be updated, and install them accordingly.

Package managers are included with most Linux distributions, and each distribution typically provides its own default tool. For example, Ubuntu uses APT (Advanced Package Tool) as its built-in package manager.

Now we can navigate to a new terminal window and type

```shell
sudo apt search openjdk
```

However in Ubuntu we can also just write the software we want to install and it will give us the package containing that name

```shell
java
```

## Repositories

When installing software with apt, a natural question arises: Where do these packages come from, and how does the package manager find them?

Linux distributions use software repositories, which are structured storage locations containing thousands of packages. These repositories are typically hosted on remote servers, though local repositories can also be configured when needed.

The package manager retrieves packages directly from these repositories, ensuring they are authentic, up-to-date, and compatible with the system.

On a fresh Linux installation, it is good practice to update the local package index so the system is aware of the latest available packages and versions. This is done using:

```shell
sudo apt update
```

### Ubuntu Software Center

On Linux we will mostly use apt package managers. However, we may need other resources as some packages are not available in those official repositories.

It could be also that the latest version could not be present.

In those cases we have some alternatives. One of those alternatives are Ubuntu Software Center

### Snap Packages

Other option we have is snap. Benefits of snaps are:

- Snap uses self contained packages. It contains all the packages contained inside.
- Everything you need to run the application is in one full package
