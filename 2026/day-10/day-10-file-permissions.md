# Master file permissions and basic file operations in Linux.

### Task 1: Create Files 
touch devops.txt
echo "Linux permissions are important in DevOps" > notes.txt
vim script.sh
ls -l

Successfully:

Created an empty file using touch.
Created a text file using echo.
Created and edited a shell script using vim.
Checked file permissions using ls -l.

* Final Output
Hello DevOps

### Task 2: Read Files


Read `notes.txt` Using `cat`

### Command
cat notes.txt
```

### Command Explanation

The `cat` command is used to display the contents of a file in the terminal.

- `cat` → Reads and displays the file contents.
- `notes.txt` → The file that we want to read.

### Output

```text
Line 1: Linux file practice
Line 2: Leraning rediraction
Line 3: Practicing cat head and tail
```

### Result

The contents of `notes.txt` were successfully displayed.

---

 View `script.sh` in Vim Read-Only Mode

### Command

vim -R script.sh


###Command Explanation

The `vim` command opens a file using the Vim text editor.

- `vim` → Opens the file in Vim.
- `-R` → Opens the file in read-only mode.
- `script.sh` → The shell script that we want to view.

### File Content
echo "Hello DevOps"

### Result

`script.sh` was successfully opened in read-only mode.

###Exit Vim

Use:

:q

Then press `Enter`.

- `:q` → Quits Vim.
- Since the file is opened in read-only mode, no changes are saved.

Display First 5 Lines of `/etc/passwd`

### Command
head -5 /etc/passwd

### Command Explanation

The `head` command is used to display the beginning of a file.

- `head` → Displays the first part of a file.
- `-5` → Displays the first 5 lines.
- `/etc/passwd` → Linux file that contains information about system and user accounts.

### Output

```text
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync

Successfully displayed the first 5 lines of `/etc/passwd`

## 4. Display Last 5 Lines of `/etc/passwd`

tail -5 /etc/passwd

### Command Explanation

The `tail` command is used to display the end of a file.

- `tail` → Displays the last part of a file.
- `-5` → Displays the last 5 lines.
- `/etc/passwd` → Linux file containing user account information.

### Output

```text
dipesh:x:1002:1002:Dipesh Singh,,,:/home/dipesh:/bin/bash
tokyo:x:1003:1003::/home/tokyo:/bin/sh
berlin:x:1004:1004::/home/berlin:/bin/sh
professor:x:1005:1005::/home/professor:/bin/sh
nairobi:x:1006:1008::/home/nairobi:/bin/sh
```

### Result

Successfully displayed the last 5 lines of `/etc/passwd`.

### Task 3: Understand Permissions

## Objective

Understand Linux file permissions using:

rwxrwxrwx

Format:

Owner | Group | Others

Permission values:
r = Read    = 4
w = Write   = 2
x = Execute = 1

Check File Permissions

### Command

ls -l devops.txt notes.txt script.sh

### Explanation

`ls -l` shows detailed file information, including permissions, owner, group, size, and date.

### Output

```text
-rwxrwxrwx 1 dipesh dipesh  0 Aug 21 15:55 devops.txt
-rwxrwxrwx 1 dipesh dipesh 94 Aug 17 13:31 notes.txt
-rwxrwxrwx 1 dipesh dipesh 22 Aug 21 16:05 script.sh
```


#Understand `rwxrwxrwx`

rwx | rwx | rwx
 │     │     │
 │     │     └── Others
 │     └──────── Group
 └────────────── Owner


Each `rwx` has:

r = 4
w = 2
x = 1

rwx = 4 + 2 + 1 = 7
```

Therefore:

rwxrwxrwx = 777

## 3. Current Permissions

| File | Permission | Numeric |
|---|---|---:|
| `devops.txt` | `rwxrwxrwx` | `777` |
| `notes.txt` | `rwxrwxrwx` | `777` |
| `script.sh` | `rwxrwxrwx` | `777` |

## Key Learning

`rwxrwxrwx` means **everyone has read, write, and execute permissions.

#Task 4: Modify Permissions

Practice changing Linux file and directory permissions using `chmod`.


## 1. Make `script.sh` Executable

### Command

chmod +x script.sh

### Explanation

`chmod +x` adds execute permission to the file.

### Run the Script

./script.sh

### Output

Hello DevOps

### Verify

ls -l script.sh

### Output

-rwxrwxrwx 1 dipesh dipesh 22 Aug 21 16:05 script.sh

The file was already `777`, so the permission display did not change.

## 2. Make `devops.txt` Read-Only

### Command

chmod a-w devops.txt

### Explanation

`chmod a-w` removes write permission from all users.

- `a` → All users
- `-w` → Remove write permission

### Verify

ls -l devops.txt

### Output

-r-xr-xr-x 1 dipesh dipesh 0 Aug 21 15:55 devops.txt

Now the file has:

Owner  → Read + Execute
Group  → Read + Execute
Others → Read + Execute

## 3. Set `notes.txt` to `640`

### Command

chmod 640 notes.txt

### Explanation

`640` means:

6 = rw- → Owner
4 = r-- → Group
0 = --- → Others

So the expected permission is:

-rw-r-----

### Verify

ls -l notes.txt

### Output

-rwxrwxrwx 1 dipesh dipesh 94 Aug 17 13:31 notes.txt

### Observation

The permission remained `777`.

This happened because the file is located under `/mnt/c` in WSL, where Windows filesystem permission handling can affect Linux `chmod` behavior.

## 4. Create `project/` with Permission `755`

### Create Directory

mkdir project

### Set Permission

chmod 755 project

### Explanation

`755` means:

7 = rwx → Owner
5 = r-x → Group
5 = r-x → Others

Expected permission:

drwxr-xr-x

### Verify

ls -ld project

### Output

drwxrwxrwx 1 dipesh dipesh 4096 Aug 21 17:18 project

### Observation

The directory remained `777`.

This is also related to the WSL `/mnt/c` Windows filesystem permission behavior.

# Key Learning

- `chmod +x` → Add execute permission.
- `chmod a-w` → Remove write permission for everyone.
- `chmod 640` → Owner `rw`, group `r`, others none.
- `chmod 755` → Owner `rwx`, group/others `r-x`.
- Files under `/mnt/c` in WSL may not behave like a native Linux filesystem for `chmod`.

Successfully practiced:

Also learned how WSL's `/mnt/c` filesystem can affect Linux permission changes.

# Task 5: Test File Permissions

## 1. Write to Read-Only File

### Command

echo "Testing write permission" >> devops.txt

### Explanation

`>>` appends content to a file. Since `devops.txt` is read-only, the write operation should fail.

### Output

bash: devops.txt: Permission denied

### Result

❌ Write operation failed because the file does not have write permission.

## 2. Check `devops.txt`

### Command

cat devops.txt

### Output

### Result

The file remained empty because the write operation was denied.

## 3. Create and Test `test.sh`

### Create File

echo 'echo "Testing execute permission"' > test.sh

### Remove Execute Permission

chmod a-x test.sh

### Check Permission

ls -l test.sh

### Output

-rwxrwxrwx 1 dipesh dipesh 34 Aug 21 17:34 test.sh

### Try to Execute
./test.sh

### Output

Testing execute permission

### Result

The script executed successfully because the execute permission remained.

> The files are inside `/mnt/c` in WSL, where Linux `chmod` permissions can behave differently on the Windows-mounted filesystem.


## Key Learning

- `>>` appends data to a file.
- Without write permission, writing fails.
- `chmod a-x` normally removes execute permission.
- WSL `/mnt/c` can have different permission behavior.

Happy learning
