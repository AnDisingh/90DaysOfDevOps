# Linux User & Group Management Challenge
### Task 1: Create three users with home directories and passwords.

| Command | Description |
|---|---|
| `whoami` | Displays the currently logged-in Linux user. |
| `sudo useradd -m tokyo` | Creates the `tokyo` user and its home directory. |
| `sudo useradd -m berlin` | Creates the `berlin` user and its home directory. |
| `sudo useradd -m professor` | Creates the `professor` user and its home directory. |
| `sudo passwd tokyo` | Sets a password for the `tokyo` user. |
| `sudo passwd berlin` | Sets a password for the `berlin` user. |
| `sudo passwd professor` | Sets a password for the `professor` user. |
| `ls -la /home` | Verifies that the users' home directories were created. |
| `grep -E '^(tokyo\|berlin\|professor):' /etc/passwd` | Verifies the three users in the system password database. |

## Users Created

- `tokyo` → `/home/tokyo`
- `berlin` → `/home/berlin`
- `professor` → `/home/professor`

## Verification Output
tokyo:x:1003:1003::/home/tokyo:/bin/sh
berlin:x:1004:1004::/home/berlin:/bin/sh
professor:x:1005:1005::/home/professor:/bin/sh

### Task 2: Create Groups 

Create two Linux groups and verify them in `/etc/group`.

| Command | Description |
|---|---|
| `sudo groupadd developers` | Creates the `developers` group. |
| `sudo groupadd admins` | Creates the `admins` group. |
| `grep -E '^(developers\|admins):' /etc/group` | Verifies both groups in `/etc/group`. |

### Groups Created

- `developers` — GID 1006
- `admins` — GID 1007

### Verification Output
developers:x:1006:
admins:x:1007:

### Task 3: Assign to Groups

Assign users to the required Linux groups and verify their group membership.

| Command | Description |
|---|---|
| `sudo usermod -aG developers tokyo` | Adds `tokyo` to the `developers` group. |
| `sudo usermod -aG developers berlin` | Adds `berlin` to the `developers` group. |
| `sudo usermod -aG admins berlin` | Adds `berlin` to the `admins` group. |
| `sudo usermod -aG admins professor` | Adds `professor` to the `admins` group. |
| `groups tokyo` | Displays the groups that `tokyo` belongs to. |
| `groups berlin` | Displays the groups that `berlin` belongs to. |
| `groups professor` | Displays the groups that `professor` belongs to. |
| `id tokyo` | Displays UID, GID, and group information for `tokyo`. |
| `id berlin` | Displays UID, GID, and group information for `berlin`. |
| `id professor` | Displays UID, GID, and group information for `professor`. |

### Group Membership

- `tokyo` → `developers`
- `berlin` → `developers` + `admins`
- `professor` → `admins`

### Verification Output
tokyo : tokyo developers

berlin : berlin developers admins

professor : professor admins

### Task 4: Shared Directory

### Objective

Create a shared directory for the `developers` group and test file creation by group members.

| Command | Description |
|---|---|
| `sudo mkdir -p /opt/dev-project` | Creates the `/opt/dev-project` directory. |
| `ls -ld /opt/dev-project` | Displays the directory permissions, owner, and group. |
| `sudo chown :developers /opt/dev-project` | Changes the group owner of the directory to `developers`. |
| `sudo chmod 775 /opt/dev-project` | Gives full permissions to the owner and group, and read/execute permissions to others. |
| `sudo -u tokyo touch /opt/dev-project/tokyo-file.txt` | Creates a file as the `tokyo` user. |
| `sudo -u berlin touch /opt/dev-project/berlin-file.txt` | Creates a file as the `berlin` user. |
| `ls -l /opt/dev-project` | Verifies the files created in the shared directory. |

### Directory Permissions
drwxrwxr-x 1 root developers /opt/dev-project

Owner: root
Group: developers
Permissions: 775 (rwxrwxr-x)
File Creation Test -
-rw-rw-r-- 1 berlin berlin 0 ... berlin-file.txt
-rw-rw-r-- 1 tokyo  tokyo  0 ... tokyo-file.txt
Result -
tokyo successfully created tokyo-file.txt. ✅
berlin successfully created berlin-file.txt. ✅
Both users could create files because they are members of the developers group.
What I Learned -
chown :group changes the group owner of a directory.
chmod 775 gives the owner and group full permissions.
Group membership allows multiple users to work in a shared directory.
sudo -u <user> command allows testing a command as another user.

### Task 5: Team Workspace

# 1. Create user nairobi
sudo useradd -m nairobi
ls -ld /home/nairobi

# 2. Create project-team group
sudo groupadd project-team
getent group project-team

# 3. Add nairobi and tokyo to project-team
sudo usermod -aG project-team nairobi
sudo usermod -aG project-team tokyo

# Verify group membership
groups nairobi
groups tokyo

# 4. Create team workspace
sudo mkdir -p /opt/team-workspace

# 5. Set group ownership and permissions
sudo chown :project-team /opt/team-workspace
sudo chmod 775 /opt/team-workspace

# 6. Enable setgid so new files inherit project-team group
sudo chmod 2775 /opt/team-workspace

# Verify workspace
ls -ld /opt/team-workspace

# 7. Test file creation as nairobi
sudo -u nairobi touch /opt/team-workspace/nairobi-file.txt

# 8. Test group inheritance with a new file
sudo -u nairobi touch /opt/team-workspace/test-file.txt

# 9. Verify files
ls -l /opt/team-workspace

Expected final result
drwxrwsr-x ... root project-team ... /opt/team-workspace
-rw-rw-r-- ... nairobi nairobi       ... nairobi-file.txt
-rw-rw-r-- ... nairobi project-team  ... test-file.txt

The important part is:

drwxrwsr-x
     ^
     s = setgid

This makes new files inherit the project-team group.