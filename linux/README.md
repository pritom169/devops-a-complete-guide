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

### Differences between APT and SNAP

- Snap packages are self contained whereas dependencies in apt is distributed into different directories
- Snap Supports universal Linux packages (package type .snap), whereas apt supports only specific linux distributions (package type .deb)
- Snap also does automatic updates for it's packages whereas packages installed via apt needs to be updated manually updated
- Since snap used self contained packages, the similar dependencies that could have been shared via multiple packages, get download multiple times. As a result, snap packages requires more disk space than apt.

### PPA (Personal Package Archive)

We have another option for package installation. A PPA (Personal Package Archive) is a software repository hosted on Launchpad that allows developers and individuals to distribute their own Ubuntu packages directly to users. PPAs are commonly used when software is not available in Ubuntu’s official repositories or when a developer wants to provide newer, experimental, or custom versions of a package. By adding a PPA to the system, users can install and update software through APT just like official packages, but the responsibility for maintenance, security, and compatibility lies with the PPA creator—not with Ubuntu.

## In a nutshell

- The first choice will always be apt. It should be used whenever possible.
- If that is not possible, the first alternative is snap package manager.
- Otherwise we can also add custom repositories.

## Package Manager for other Linux distributions

There are two major linux distributions type. They are followings:

- Debian based (Ubuntu, Debian, Mint) - In debian based operating systems, apt is the primary package manager
- Red Hat based (RHEL, CentOS, Fedora) - In Red Hat based operating systems, yum is primarily used

# Vim Editor

Vim is a highly efficient and lightweight text editor that comes preinstalled on almost all Linux systems, making it especially valuable for editing files on servers or over SSH where graphical tools are unavailable. Its fully keyboard-driven workflow allows for fast navigation and editing. To open a file in VIM one can type `vim <filename-with-extension>`

Vim has two modes:

1. **Command Mode (default):** In the command mode the user can move around, see, copy text and write some command.

2. **Insert Mode (primarily for typing text):**

- `i` - for typing texts normally
- `Esc` - stop typing and go back to normal mode
- `:w`- to save
- `:q` - to quit
- `:wq` - to save and quit
- `:q!` - quit without saving

Even more useful commands to know:

- `vim <filename-with-extension>` - to create and open a file
- `dd` - to remove the whole line
- `d5` - to delete 5 lines
- `u` - to undo the previous action
- `A` - type capital A to jump to the end of the line and switch to insert mode from command mode
- `$` - type `$` to jump to the end of the line and it will not switch to insert mode
- `0` - type 0 to jump to the beginning of the line and it will also not switch to insert mode
- `100G` - to jump to line 100

File searching commands:

- command mode + `/<search-text>` - to search a text inside the document
- command mode + `n` - to navigate to the next searched match
- command mode + `N` - to search in opposite direction

Text replacement command:

- command mode + `:%s/old/new` - to replace an **old** text with a **new** text

# Users and Permissions

In Linux there are three user categories:

1. **Superuser Account:**
   Superuser is the root user. It has unrestricted permissions.

For administrative tasks: Need to login as Root user or execute commands as root (a sudo command)

2. **User Account:**
   A regular user we create to login. A user will have a dedicated directory inside `home`

3. **Service Account:**
   A Linux service account is a special type of user account created specifically for running system services, daemons, and background processes—not for human login.

Each service (like nginx, mysql, apache, postgres) often has its own dedicated user account. Since the account has very restricted permissions, even if a service is hacked, the attacker gets minimal access. It also Keeps system processes separate from human users.

## Windows vs Linux (User Account)

Both Windows and Linux allow multiple user accounts on the same device, and these accounts are stored on the system — not tied to the hardware. In Linux, user accounts are typically local unless the machine uses centralized authentication (e.g., LDAP or Active Directory). Removing the system’s storage would remove the user accounts simply because the account database lives on the disk, not because they are tied to hardware.

## Multiple Users on a Server

For Linux having Multi-User is important for servers. Now you might be asking, why not just use a standard user? In other words, why not a shared user?

In a shared teams with variability of skills, different users should have different permission as it will allow for more traceability.

# Users and Groups

As the team size increases managing individual responsibility by a single admin becomes difficult.

To solve this in a linux way, it's a best practice to create some group.

For example a person from a DevOps group can have different set of permissions than in an admin group. So when a person leaves, rather than deleting all of his permissions, the admin can just remove him from the groups. In this way, user management becomes a bit easier.

### User management in Practice

As we know /etc holds system wide configuration, it also holds user account information.

We can simply write `cat  /etc/passwd` on terminal in order to see all the users. In the file we can see each user has exactly one line of information. Here is the pattern they follow:

```
USERNAME: PASSWORD : UID : GID :GECOS : HOMEDIR : SHELL
```

- User ID (UID): Each user has a unique ID. UID 0 is reserved for root
- Group ID (GID): The primary group ID (stored in /etc/group file). Each user will have a primary group and their ID will be stored here.
- Home Directory (HOMEDIR): Home directory of the user
- Default shell (SHELL): Absolute path of a shell

#### Add a new user

To create a new user, run:

```shell
sudo adduser tom
```

After the account is created, you can verify its presence by checking the system’s user database:

```shell
cat /etc/passwd
```

When a new user is added, a primary group with the same name is automatically created and assigned as the user’s default group.

To set or update Tom’s password, use:

```shell
sudo passwd tom
```

To switch to the newly created user account, run:

```shell
su - tom
```

#### Creating Groups

Managing permissions for individual users quickly becomes inefficient as the number of users grows. To simplify administration, Linux uses groups—allowing users with similar responsibilities to share a common set of permissions.

To create a new group named `devops`, run:

```shell
sudo groupadd devops
```

You can view all existing groups on the system by checking the `/etc/group` file:

```shell
cat /etc/group
```

On the internet you might see `adduser`, `addgroup`, `deluser`, and `delgroup`

Also you might see `useradd`, `groupadd`, `userdel`and `groupdel`

- When working manually, use adduser and addgroup because they are simpler, interactive, and provide sensible defaults.

- When writing automation or scripts, use useradd and groupadd because they are non-interactive, stable, and require explicit configuration.

#### Change of Groups

To modify a user’s primary group, we can use the `usermod` command. The following example assigns the `devops` group as Tom’s new primary group:

```shell
sudo usermod -g devops tom
```

After changing the primary group, Tom’s original primary group becomes unused. Since it is no longer needed, we can safely delete it:

```shell
sudo delgroup tom
```

- -G → set secondary groups (overwrite existing ones)
- admin → the only secondary group Tom will belong to after this

Now let's replace Tom's entire list of secondary groups with the group `admin`.

```shell
sudo usermod -G admin tom
```

- -a → append (only works together with -G)
- -G newgroup → add him to this additional secondary group

Let's assume we don't want to remove all the secondary groups. This command adds Tom to the group newgroup without removing his current group memberships.

```shell
sudo usermod -aG newgroup tom
```

To view the groups the current user belongs to, run:

```shell
groups
```

To check the group memberships of another user—for example, Tom—use:

```shell
groups tom
```

### User Creation with Group Assignment

We can create a new user and simultaneously assign them to a group. The following command creates a user named `nicole` and adds her to the `devops` group as a secondary group:

```shell
sudo useradd -G devops nicole
```

To remove a user from a group, we can use the `gpasswd` command. The example below removes `nicole` from the `devops` group:

```shell
sudo gpasswd -d nicole devops
```

# File Permissions and Ownership

In Linux, almost everything is treated as a file. Because of this, permissions revolve around determining who can read, write, or execute these files.

To begin, here are some familiar commands:

```shell
ls         # list all files in the current directory
ls -a      # list all files, including hidden ones
```

To view files along with their detailed permissions, ownership, and metadata, use:

```shell
ls -l
```

## Ownership

Ownership defines who controls a file and what actions they are allowed to perform. Every file in Linux has two types of owners:

1. **User (Owner):** The specific user who owns the file.
2. **Group:** A group of users who share access permissions to the file.
3. The user
4. The group

### Change of Ownership

File ownership in Linux can be modified at both the user and group level. The following examples demonstrate how to change each type of ownership.

To change the **user (owner)** of a file, use the `chown` command:

```shell
sudo chown admin text.txt
```

- **sudo** — Executes the command with elevated (root) privileges, which are required for modifying ownership.
- **chown** — Stands for _change owner_ and is used to assign a new user as the file owner.
- **admin** — The new owner of the file.
- **text.txt** — The file whose ownership is being updated.

To change the **group ownership** of a file, use the `chgrp` command:

```shell
sudo chgrp devops text.txt
```

- **chgrp** — Stands for _change group_ and updates the group associated with the file.
- **devops** — The new group assigned to the file.
- **text.txt** — The file whose group ownership is being modified.

#### Analyzing permissions

When we type `ls -l` we see the following commands:

```shell
drwxrwxr-x 2 mypritux mypritux 4096 Nov 21 11:44 app
-rw-rw-r-- 1 mypritux mypritux  107 Nov 16 19:20 config.yaml
-rw-rw-r-- 1 tom      devops      0 Nov 20 08:49 text.txt
```

#### Linux Permission Vocabulary

| Symbol              | Meaning                | Applies To             | Description                                                      |
| ------------------- | ---------------------- | ---------------------- | ---------------------------------------------------------------- |
| **d**               | Directory              | File Type              | The item is a directory.                                         |
| **-**               | Regular File           | File Type              | A normal file (text, binary, etc.).                              |
| **l**               | Symbolic Link          | File Type              | A shortcut-like reference to another file.                       |
| **r**               | Read                   | Owner / Group / Others | Allows viewing the file or listing folder contents.              |
| **w**               | Write                  | Owner / Group / Others | Allows modifying a file or adding/removing files in a directory. |
| **x**               | Execute                | Owner / Group / Others | Allows running a file or entering a directory.                   |
| **---**             | No Permission          | Owner / Group / Others | Access is not granted.                                           |
| **rwx**             | Full Permission        | Owner / Group / Others | Read + write + execute.                                          |
| **rw-**             | Read + Write           | Owner / Group / Others | Modify and read, but cannot execute.                             |
| **r-x**             | Read + Execute         | Owner / Group / Others | View + execute, cannot modify.                                   |
| **r--**             | Read Only              | Owner / Group / Others | Only view.                                                       |
| **w--**             | Write Only             | Owner / Group / Others | Modify without viewing (rare).                                   |
| **x--**             | Execute Only           | Owner / Group / Others | Execute/enter but cannot read.                                   |
| **Hard Link Count** | A number like `1`, `2` | Metadata               | Number of directory entries pointing to the file.                |

---

#### Understanding Permission Positions

##### Permission Position Table

| Position | Characters | Meaning                                                   |
| -------- | ---------- | --------------------------------------------------------- |
| **1**    | `d`        | File type (`d` = directory, `-` = file, `l` = link, etc.) |
| **2–4**  | `rwx`      | Owner permissions                                         |
| **5–7**  | `rwx`      | Group permissions                                         |
| **8–10** | `r-x`      | Others (public) permissions                               |

---

##### Expanded Breakdown

| Position | Character | Description                          |
| -------- | --------- | ------------------------------------ |
| **1**    | `d`       | Directory                            |
| **2**    | `r`       | Owner can read                       |
| **3**    | `w`       | Owner can write                      |
| **4**    | `x`       | Owner can execute / enter directory  |
| **5**    | `r`       | Group can read                       |
| **6**    | `w`       | Group can write                      |
| **7**    | `x`       | Group can execute / enter directory  |
| **8**    | `r`       | Others can read                      |
| **9**    | `-`       | Others cannot write                  |
| **10**   | `x`       | Others can execute / enter directory |

##### Modifying Executable Permissions

First, we will revoke executable permissions for the `app` directory. Note that for directories, removing the executable bit prevents users from traversing (entering) that directory.

```shell
sudo chmod -x app
```

##### Targeting Specific Ownership Classes

To modify permissions for a specific entity (User, Group, or Others) rather than applying the change globally, you must specify the ownership class prior to the operator.

For example, to remove write permission specifically for the group, use the g designator:

```shell
sudo chmod g-w config.yaml
```

| **Category**     | **Symbol** | **Description**                                | **Octal Value** |
| ---------------- | ---------- | ---------------------------------------------- | --------------- |
| **User Classes** |            |                                                |                 |
| Owner            | `u`        | File/directory owner                           | -               |
| Group            | `g`        | Group members                                  | -               |
| Other            | `o`        | All other users                                | -               |
| All              | `a`        | All users (u+g+o)                              | -               |
| **Operations**   |            |                                                |                 |
| Add              | `+`        | Grant permission                               | -               |
| Remove           | `-`        | Revoke permission                              | -               |
| Set exactly      | `=`        | Set exact permissions                          | -               |
| **Permissions**  |            |                                                |                 |
| Read             | `r`        | View file contents / List directory            | 4               |
| Write            | `w`        | Modify file / Create/delete files in directory | 2               |
| Execute          | `x`        | Run file / Access directory                    | 1               |
| No permission    | `-`        | No access                                      | 0               |

**Common Examples:**

- `chmod u+x file` - Add execute permission for owner
- `chmod go-w file` - Remove write permission for group and others
- `chmod g=rwx directory` - Give read, write and executable permission to the directory
- `chmod g=rw- directory` - Give read, write, and remove executable permission to the directory

##### Change permission with digits

| **Octal** | **Binary** | **Symbolic** | **Owner**              | **Group**       | **Other**      | **Description**                  |
| --------- | ---------- | ------------ | ---------------------- | --------------- | -------------- | -------------------------------- |
| **0**     | 000        | `---`        | No permissions         | No permissions  | No permissions | No access at all                 |
| **1**     | 001        | `--x`        | No permissions         | No permissions  | Execute only   | Others can execute/traverse only |
| **2**     | 010        | `-w-`        | No permissions         | Write only      | No permissions | Group can modify only (rare)     |
| **3**     | 011        | `-wx`        | No permissions         | Write + Execute | No permissions | Group can modify and execute     |
| **4**     | 100        | `r--`        | Read only              | No permissions  | No permissions | Owner can view only              |
| **5**     | 101        | `r-x`        | Read + Execute         | No permissions  | No permissions | Owner can view and execute       |
| **6**     | 110        | `rw-`        | Read + Write           | No permissions  | No permissions | Owner can view and modify        |
| **7**     | 111        | `rwx`        | Read + Write + Execute | No permissions  | No permissions | Owner has full permissions       |

| **Three-Digit Octal** | **Symbolic** | **Description**                     | **Common Use Case**                       |
| --------------------- | ------------ | ----------------------------------- | ----------------------------------------- |
| **000**               | `----------` | No permissions for anyone           | Inaccessible file                         |
| **400**               | `-r--------` | Owner read only                     | Protected read-only file                  |
| **600**               | `-rw-------` | Owner read/write only               | Private files (SSH keys, passwords)       |
| **640**               | `-rw-r-----` | Owner read/write, group read        | Shared read-only with group               |
| **644**               | `-rw-r--r--` | Owner read/write, others read       | Public readable files (documents, images) |
| **660**               | `-rw-rw----` | Owner and group read/write          | Collaborative files within group          |
| **664**               | `-rw-rw-r--` | Owner/group read/write, others read | Shared editable, publicly readable        |
| **666**               | `-rw-rw-rw-` | Everyone read/write                 | World-writable (insecure, avoid)          |
| **700**               | `-rwx------` | Owner full access only              | Private scripts and programs              |
| **750**               | `-rwxr-x---` | Owner full, group execute           | Scripts for group execution               |
| **755**               | `-rwxr-xr-x` | Owner full, others execute/read     | Public executables, web directories       |
| **770**               | `-rwxrwx---` | Owner/group full access             | Shared project directories                |
| **775**               | `-rwxrwxr-x` | Owner/group full, others execute    | Collaborative executables                 |
| **777**               | `-rwxrwxrwx` | Everyone full access                | World-writable/executable (dangerous!)    |

| **Special Permissions**   | **Octal**    | **Effect on Files**                     | **Effect on Directories**         |
| ------------------------- | ------------ | --------------------------------------- | --------------------------------- |
| Setuid (`s` in owner)     | **4000**     | Execute as file owner                   | No effect                         |
| Setgid (`s` in group)     | **2000**     | Execute as group owner                  | New files inherit directory group |
| Sticky bit (`t` in other) | **1000**     | No effect                               | Only owner can delete their files |
| **Combined Examples:**    |              |                                         |                                   |
| `4755`                    | `-rwsr-xr-x` | Setuid executable (runs as owner)       | `/usr/bin/passwd`                 |
| `2755`                    | `-rwxr-sr-x` | Setgid executable (runs as group)       | Shared collaboration tools        |
| `1777`                    | `drwxrwxrwt` | Sticky directory (public but protected) | `/tmp` directory                  |

## Piping Commands

In Linux, a pipe (`|`) allows the output of one command to be passed directly as the input to another. For example, the command below sends the contents of the system log to `less`, making it easier to scroll through and read:

```shell
cat /var/log/syslog | less
```

You can also view your previously executed commands in a readable, scrollable format by piping the command history into `less`:

```shell
history | less
```

### Piping and Grepping

We have already seen how to view command history in a structured and readable format. To further refine the output, we can use `grep`—a powerful tool for searching text based on patterns. For example, the following command filters the history to display only entries that include the keyword `sudo`:

```shell
history | grep sudo
```

To narrow the results even more, we can search for multiple terms. The example below retrieves only those history entries that contain both `sudo` and the `chmod` command:

```shell
history | grep "sudo chmod"
```

We can also use `grep` to check whether a specific keyword appears within a file. For instance, to find lines in `config.yaml` that contain the word `Deployment`, run:

```shell
cat config.yaml | grep Deployment
```

### Redirects

Redirects in Linux allow you to control where a command reads its input from and where it sends its output. Instead of displaying results on the terminal, you can redirect them into a file for storage, processing, or review.

```shell
history | grep sudo > sudo-commands.txt
```

We can also use redirection to copy the contents of one file into another:

```shell
cat sudo-commands.txt > sudo-rm-commands.txt
```

Redirection is also useful when filtering data and saving the results to a new file. The following examples show the difference between overwriting a file and appending to it:

```shell
history | grep rm > sudo-rm-commands.txt   # Overwrites the file with the new content

history | grep rm >> sudo-rm-commands.txt  # Appends the new content to the end of the file
```

## Shell Scripting

Shell scripting is the practice of writing a sequence of Linux commands in a file so they can be executed automatically by the shell. It is commonly used to automate repetitive tasks, combine multiple commands into workflows, process files, and create lightweight programs without requiring a full programming language.

### Types of Shells

Linux systems provide different shell programs, each offering its own features and capabilities:

1. **sh (Bourne Shell) — `/bin/sh`**  
   The original Unix shell, widely supported and historically used as the default on older Linux distributions.

2. **Bash (Bourne Again Shell) — `/bin/bash`**  
   An enhanced and more feature-rich version of the Bourne shell. Bash is the default shell on most modern Linux and UNIX‑like systems and is the most commonly used shell today.

In everyday usage, the terms _shell_ and _Bash_ are often used interchangeably. Bash functions as a complete scripting language, enabling the creation of robust and flexible shell scripts.

### Shebang

A common question is: **If all shell scripts use the `.sh` extension, how does the operating system know which shell to execute them with?**

Different scripts may be written for different shells—such as **Bash**, **sh**, or **Zsh**. To ensure the correct interpreter is used, we explicitly specify it.

This is done using a **shebang**.  
A **shebang** is the very first line of a script and tells the operating system which interpreter should execute the file.

For example, after creating a script with:

```shell
touch setup.sh
```

You can specify Bash as the interpreter by adding this at the top of the file:

```shell
#!/bin/bash
```

> Why is it called a “shebang”?

> The # symbol is often read as sharp, and ! is traditionally called bang.
> Combined, sharp-bang became shebang.

### Bash Concepts

#### Variables

Bash variables allow you to store and reuse values within a script. They do not require explicit data types, and by convention, variable names use uppercase for constants and lowercase for regular variables. To reference a variable, prefix its name with a `$`.

```bash
name="Pritom"
age=100
echo "Hello, I am $name, and I am $age years old"
```

##### Reading User Input

Bash provides the `read` command to capture input from the user during script execution.

```bash
echo "What's your favorite color?"
read color
echo "You chose: $color"
```

#### Command Substitution

Command substitution allows you to execute a command and use its output as a value inside another command or variable. It is commonly used to capture dynamic information—such as dates, system data, or command results—directly within scripts.

```bash
current_date=$(date +%Y-%m-%d)
user=$(whoami)
echo "Today is $current_date and you are $user"
```

#### Conditionals

Conditionals in Bash allow a script to make decisions based on whether a condition is true or false. The `if` statement is commonly used to check file types, compare values, or verify the status of commands.

```bash
#!/bin/bash
filename="test.txt"

if [ -f "$filename" ]; then
	echo "$filename exists"
else
	echo "$filename does not exists"
fi
```

#### Loops

Arrays in Bash allow you to store multiple values in a single variable. Using a `for` loop, you can iterate through each element and perform actions on them.  
The example below demonstrates iterating through an array of fruits and printing a message for each one:

```bash
fruits=("apple" "banana" "cherry" "date")

for fruit in ${fruits[@]}; do
        echo "I like $fruit"
done
```

#### Functions

```bash
greet() {
	local person=$1
	echo "Hello, $person!"
}

add_numbers(){
	local sum=$(($1 + $2))
	echo $sum
}

greet "Pritom"
result=$(add_numbers 5 7)
echo "5 + 7 = $result"
```

#### Case Statements

Case statements in Bash are more like switch statements in other languages like C, and Java.

```bash
fruit="apple"
case $fruit in
    apple)
        echo "It's an apple"
        ;;
    banana)
        echo "It's a banana"
        ;;
    *)
        echo "Unknown fruit"
        ;;
esac
```

#### String Operations

```bash
text="Hello World"
echo "Length: ${#text}"
echo "Uppercase: ${text^^}"
echo "Lowercase: ${text,,}"
echo "Substring: ${text:0:5}"
echo "Replace: ${text/World/Bash}"
```

#### File Operations

```bash
# Create a file
echo "Sample content" > output.txt

# Append to file
echo "More content" >> output.txt

# Read file line by line
while IFS= read -r line; do
    echo "Line: $line"
done < output.txt

# Check if the file exists
if [ -e "output.txt" ]; then
        echo "File Exists"
fi
```
