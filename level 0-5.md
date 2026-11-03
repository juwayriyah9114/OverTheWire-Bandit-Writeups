# OverTheWire: Bandit Write-ups (Levels 0 – 5)

This repository tracks my solutions, commands, and key takeaways while working through the **OverTheWire Bandit** wargame.

---

## Level 0 → Level 1
* **Objective:** Connect to the game server via SSH and retrieve the first password.
* **Command:**
  ```bash
  ssh bandit0@bandit.labs.overthewire.org -p 2220
  ls -a
  cat readme
  ```
* **Password:** `<REDACTED>`

---

## Level 1 → Level 2
* **Objective:** Read a file named `-` in the home directory.
* **Command:**
  ```bash
  cat ./-
  ```
* **Key Concept:** 
  A leading hyphen (`-`) makes Linux utilities interpret the filename as a command-line flag or option (e.g., `-n` or `-s`). Prepending `./` explicitly specifies the relative path to avoid this ambiguity.

---

## Level 2 → Level 3
* **Objective:** Read a file with spaces in the filename (`spaces in this filename`).
* **Command:**
  ```bash
  cat -- "spaces in this filename"
  # Alternatively: cat spaces\ in\ this\ filename
  ```
* **Key Concept:** Filenames with spaces must be enclosed in double quotes or escaped using backslashes `\` so the shell treats the entire string as a single argument.

---

## Level 3 → Level 4
* **Objective:** Find the hidden file inside the `inhere` directory.
* **Command:**
  ```bash
  cd inhere
  ls -la
  cat .hidden
  ```
* **Key Concept:** In Linux, files starting with a dot (`.`) are hidden by default. Use `ls -a` or `ls -la` to list all hidden contents.

---

## Level 4 → Level 5
* **Objective:** Identify the human-readable file in the `inhere` directory among multiple ASCII/binary files.
* **Command:**
  ```bash
  cd inhere
  file ./-*
  cat ./-file07
  ```
* **Key Concept:** The `file` command determines the data type of a file (e.g., ASCII text vs. binary executable) regardless of its extension.

---

## Level 5 → Level 6
* **Objective:** Find a file matching specific criteria: human-readable, 1033 bytes in size, and non-executable.
* **Command:**
  ```bash
  find . -type f -size 1033c ! -executable
  cat <path_to_file>
  ```
* **Key Concept:** The `find` command filtering flags:
  * `-type f` restricts results to regular files.
  * `-size 1033c` matches exact byte size (`c` = bytes).
  * `! -executable` excludes executable files.
