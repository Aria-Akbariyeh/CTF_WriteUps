

```powershell

# Syntax
rsync [OPTION] … [USER@]HOST::SRC [DEST]

# List directories available to anonymous user 
rsync --list-only 10.129.228.37::

# list files/folders inside public directory
rsync --list-only 10.129.228.37::public

# Get the flag.txt file inside public directory
rsync --list-only 10.129.228.37::public/flag.txt flag.txt
```