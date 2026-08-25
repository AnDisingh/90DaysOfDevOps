# File Ownership Challenge

### Task 1: Understanding Ownership
1. Go to Home Directory

cd ~

Explanation:
cd ~ moves to the current user's home directory.

2. List Files

ls

Output:

backup.sh

Explanation:
ls displays the files and directories in the current directory.

3. Check File Ownership

ls -l

Output:

total 0
-rwxr-xr-x 1 dipesh dipesh 56 Aug 19 14:14 backup.sh

Explanation:
ls -l displays detailed information about files, including permissions, owner, group, size, and date.

-rwxr-xr-x 1 dipesh dipesh 56 Aug 19 14:14 backup.sh
             │      │
             │      └── Group
             └── Owner


 Document: What's the difference between owner and group?
In Linux, every file and directory has an owner and a group.

* Owner

The owner is the user who owns the file.

Example:

backup.sh → Owner: dipesh

The owner can have specific permissions such as read, write, and execute.

* Group

The group is a collection of users who share access permissions to the file.

Example:

backup.sh → Group: dipesh

### Task 2: Basic chown Operations
1. Create the File

Command:

touch devops-file.txt

Explanation:
Creates an empty file named devops-file.txt.

2. Check Current Owner

Command:

ls -l devops-file.txt

Output:

-rw-r--r-- 1 dipesh dipesh 0 Aug 24 13:59 devops-file.txt

Result:

Owner → dipesh
Group → dipesh
3. Check/Create tokyo User

Command:

sudo useradd -m tokyo

Output:

useradd: user 'tokyo' already exists

Explanation:
The tokyo user already exists, so there was no need to create it again.

4. Change Owner to tokyo

Command:

sudo chown tokyo devops-file.txt

Explanation:
chown means change owner. It changes the owner of the file to tokyo.

Verify:

ls -l devops-file.txt

Output:

-rw-r--r-- 1 tokyo dipesh 0 Aug 24 13:59 devops-file.txt

Result:

Owner → tokyo
Group → dipesh
5. Change Owner to berlin

Command:

sudo useradd -m berlin

Output:

useradd: user 'berlin' already exists

Explanation:
The berlin user already exists.

Now change the owner:

sudo chown berlin devops-file.txt

Verify:

ls -l devops-file.txt

Output:

-rw-r--r-- 1 berlin dipesh 0 Aug 24 13:59 devops-file.txt

Result:

Owner → berlin
Group → dipesh

### Task 3: Basic chgrp Operations
 1. Create the File
touch team-notes.txt

Explanation:
Creates an empty file named team-notes.txt.

2. Check Current Group
ls -l team-notes.txt

Output:

-rw-r--r-- 1 dipesh dipesh 0 Aug 24 14:22 team-notes.txt

Result:

Owner: dipesh
Group: dipesh
3. Create the Group

sudo groupadd heist-team

Explanation:
Creates a new Linux group named heist-team.

The command completed successfully, so the group was created.

4. Change the File Group

sudo chgrp heist-team team-notes.txt

Explanation:
chgrp means change group. It changes the group ownership of team-notes.txt from dipesh to heist-team.

5. Verify the Change

ls -l team-notes.txt

Output:

-rw-r--r-- 1 dipesh heist-team 0 Aug 24 14:22 team-notes.txt

Result:

Owner → dipesh
Group → heist-team

In this task, the owner remained dipesh, while the group changed to heist-team.

### Task 4: Combined Owner & Group Change
 1. Create the File

touch project-config.yaml

Explanation:
Creates an empty file named project-config.yaml.

2. Check Initial Ownership

ls -l project-config.yaml

Output:

-rw-r--r-- 1 dipesh dipesh 0 Aug 24 14:43 project-config.yaml

Result:

Owner: dipesh
Group: dipesh
3. Change Owner and Group Together

sudo chown professor:heist-team project-config.yaml

Explanation:
chown owner:group file changes both the owner and group in one command.

Owner → professor
Group → heist-team

Verify:

ls -l project-config.yaml

Output:

-rw-r--r-- 1 professor heist-team 0 Aug 24 14:43 project-config.yaml
4. Create the Directory

mkdir app-logs

Explanation:
Creates a directory named app-logs.

5. Change Directory Owner and Group

sudo chown berlin:heist-team app-logs

Explanation:
Changes both the owner and group of the app-logs directory.

Owner → berlin
Group → heist-team
6. Verify Directory Ownership

ls -ld app-logs

Output:

drwxr-xr-x 1 berlin heist-team 4096 Aug 24 14:49 app-logs

Result:

Owner: berlin
Group: heist-team

The -d option tells ls to show information about the directory itself.

Key Learning
Syntax:
sudo chown owner:group file

### Task 5: Recursive Ownership
 1. Create Directory Structure
mkdir -p heist-project/vault
mkdir -p heist-project/plans
touch heist-project/vault/gold.txt
touch heist-project/plans/strategy.comf

Explanation:

mkdir -p creates the required directories.
touch creates empty files.
The directory structure created was:
heist-project/
├── vault/
│   └── gold.txt
└── plans/
    └── strategy.comf

2. Create the planners Group

sudo groupadd planners

Explanation:
Creates a new Linux group named planners.

3. Change Ownership Recursively

sudo chown -R professor:planners heist-project/

4. Verify Ownership

ls -lR heist-project/

Output:

heist-project/:
total 0
drwxr-xr-x 1 professor planners 4096 Aug 24 15:14 plans
drwxr-xr-x 1 professor planners 4096 Aug 24 15:13 vault

heist-project/plans:
total 0
-rw-r--r-- 1 professor planners 0 Aug 24 15:14 strategy.comf

heist-project/vault:
total 0
-rw-r--r-- 1 professor planners 0 Aug 24 15:13 gold.txt
Result

All directories and files now have:

Owner → professor
Group → planners

**Key Learning

The -R option is used for recursive ownership changes.

sudo chown -R owner:group directory

It changes the owner and group of the directory and everything inside it.

### Task 6: Practice Challenge 
Note: The ownership test was performed in the WSL Linux home directory (~/) because the GitHub repository is located under /mnt/c/, where Windows-mounted filesystem permissions can behave differently.

1. Create Groups
sudo groupadd vault-team
sudo groupadd tech-team
Explanation
groupadd → Creates a new Linux group.
vault-team → Group for vault-related files.
tech-team → Group for technical files.
2. Create the Directory
mkdir -p bank-heist
Explanation
mkdir → Creates a directory.
-p → Creates the directory without showing an error if it already exists.
3. Create the Files
touch bank-heist/access-codes.txt
touch bank-heist/blueprints.pdf
touch bank-heist/escape-plan.txt
Explanation

touch creates empty files if they don't already exist.

Files created:

bank-heist/
├── access-codes.txt
├── blueprints.pdf
└── escape-plan.txt
4. Change File Ownership
Access codes
sudo chown tokyo:vault-team bank-heist/access-codes.txt

Owner: tokyo
Group: vault-team

Blueprints
sudo chown berlin:tech-team bank-heist/blueprints.pdf

Owner: berlin
Group: tech-team

Escape plan
sudo chown nairobi:vault-team bank-heist/escape-plan.txt

Owner: nairobi
Group: vault-team

5. Verify Ownership
ls -l bank-heist/

Expected result:

-rw-r--r-- 1 tokyo   vault-team 0 ... access-codes.txt
-rw-r--r-- 1 berlin  tech-team  0 ... blueprints.pdf
-rw-r--r-- 1 nairobi vault-team 0 ... escape-plan.txt

The important part is:

tokyo   vault-team
berlin  tech-team
nairobi vault-team
Key Commands
Command	Purpose
ls -l	View ownership and permissions
groupadd	Create a group
touch	Create an empty file
chown owner file	Change file owner
chgrp group file	Change file group
chown owner:group file	Change owner and group
chown -R owner:group directory/	Change ownership recursively
Most Important Command
sudo chown owner:group filename

Example:

sudo chown tokyo:vault-team access-codes.txt

This changes both the owner and group of the file.

What I Learned
Linux files have an owner and a group.
chown is used to change file ownership.
groupadd creates new groups.
ls -l is useful for verifying ownership and permissions.
WSL's /mnt/c filesystem can behave differently from the native Linux filesystem, so Linux permission exercises are better performed under ~/.

Happy learning.
