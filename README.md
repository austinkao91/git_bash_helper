git CLI utility tool
- work with git files without having to copy and paste/ type in filenames

###gitN
Without arguments, this command will display all unstaged changes for git, and give line numbers for each file.
```
LM-SJN-40002721:git_bash_helper akao$ gitN
     1) modified:       README.md
     2) modified:       git_bash
```

The last argument (space delimited) provided to gitN will have to be a number, and it will lookup the line number as shown above, and will utilize the last field of the line delimited by spaces.
All arguments in between will be provided as standard git options.
```
LM-SJN-40002721:git_bash_helper akao$ gitN diff 2
[INFO] Executing git with arguments diff for git_bash
diff --git a/git_bash b/git_bash
index b5c52f1..8a803b5 100644
--- a/git_bash
+++ b/git_bash
@@ -3,6 +3,7 @@
 # Git helper commands
 ######################################################

+
 function gitN() {
     getLineGen gitC git "$@"
 }

```

The last argument also has a special keyword 'all', which will perform the argument for all present lines (in reverse)
```
LM-SJN-40002721:git_bash_helper akao$ gitN add all
[INFO] Executing git with arguments add for git_bash
[INFO] Executing git with arguments add for README.md
```


### gitRN
Without arguments, this command will display all unstaged changes for git, and give line numbers for each file.
However, the difference between this and gitN, is it will allow you to review your changes and edit the files as
necessary. Unlike gitN, this command does not accept any arguments other than the line number argument.
For each file, you are able to either stage the changes, discard the changes, or edit the file
```
LM-SJN-40002721:git_bash_helper akao$ gitRN 2
[INFO] Executing gitR with arguments  for git_bash
diff --git a/git_bash b/git_bash
index b5c52f1..8a803b5 100644
--- a/git_bash
+++ b/git_bash
@@ -3,6 +3,7 @@
 # Git helper commands
 ######################################################

+
 function gitN() {
     getLineGen gitC git "$@"
 }
[INFO] Do you want to stage changes for git_bash (y/n), discard (d), or edit file (e)?
```

### getLineGen
This is the base function which accepts 3 different arguments. 
* First argument provides the display command (this is the command used to display the data with line numbers)
* Second argument provides the execute command (this command will be used to execute against specified data)
* Third argument provides the line number of the display command to use as data.'all' is a special keyword which
executes the execute command against all lines from the display command in reverse order
  * NOTE: by default, the data provided will be the last column of the line in the display command (delimited by spaces).
  Two environment variables can be provided for override. LB and UB (lowerbound and upperbound), which specify the
  columns to take from the line of the display command
  ```
  LM-SJN-40002721:git_bash_helper akao$ getLineGen "ls -l" echo
       1) total 16
       2) -rw-r--r--  1 akao  120066429  3430 Nov 22 14:04 README.md
       3) -rw-r--r--  1 akao  120066429  4071 Nov 22 13:59 git_bash
  ```
  ```
  LM-SJN-40002721:git_bash_helper akao$ getLineGen "ls -l" echo 2
  [INFO] Executing echo with arguments  for README.md
  README.md
  ```
  ```
  LM-SJN-40002721:git_bash_helper akao$ LB=2 UB=5 getLineGen "ls -l" echo 2
  [INFO] Executing echo with arguments  for -rw-r--r-- 1 akao 120066429
  -rw-r--r-- 1 akao 120066429

  ```
  
 
Sample functions defined:
```
function findN() {
    getLineGen "find . -iname '$1'" vim "$2"
}
```
In the function above, the display command is "find . -iname '$1'", so the first argument passed to this command
will be the file name to search for. The execute command is vim, and the second argument passed to this command will
be the line number which to open with vim
```
LM-SJN-40002721:git_bash_helper akao$ findN README.*
     1) ./README.md
``` 

