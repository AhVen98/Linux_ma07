# Navigating and Manipulating Directories

[Basic Ubuntu Shell Tutorial]([allo](https://ubuntu.com/tutorials/command-line-for-beginners#1-overview))

- The `pwd` command (print working directory) wil return the actual shell operation location

    ```bash
    $ pwd
    ```

- The `cd` command (change directory) will move the shell operation location 
    
    Go down a directory.
    ```bash
    $ cd [directory]
    ```

    Go up a directory.
    ```bash
    $ cd ..
    ```

    Example with relative path.
    ```bash
    $ cd desktop
    ```

- The `mkdir` command (make directory) will create a directory at a location

    ```bash
    $ mkdir [path]
    ```
    Example with absolute path.
    ```bash
    $ mkdir /tmp/exemple
    ```

- The `ls` command (list) will return the files and directories at the shell operation location

    ```bash
    $ ls
    ```

- The `touch` command will create an empty file

    ```bash
    $ touch [filename.extension]
    ```
------------------------------------------------------------ A VERIFIER SI PAS PLUS SIMPLE
- Combining `mkdir` and `touch` with `&&`. (-p parameter => creates missing directories in the path)
    ```bash
    $ mkdir -p [path] && touch [path]/[filename.extension]
    ```

