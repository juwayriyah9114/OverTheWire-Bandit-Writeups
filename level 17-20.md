## Level 17 → Level 18
* **Objective:** Identify the password for `bandit18`, which is the only line that differs between `passwords.old` and `passwords.new`.
* **Command:**
  ```bash
  ssh -i bandit17.key bandit17@bandit.labs.overthewire.org -p 2220
  ls
  diff passwords.old passwords.new
  ```
* **Key Concept:** The `diff` utility compares two files line by line, displaying `<` for lines from the first file (`passwords.old`) and `>` for the updated line in the second file (`passwords.new`).
* **Password:** `<REDACTED>`

---

## Level 18 → Level 19
* **Objective:** Retrieve the password stored in `readme` in the home directory when `.bashrc` has been modified to immediately log out interactive sessions (`Byebye!`).
* **Command:**
  ```bash
  ssh -t bandit18@bandit.labs.overthewire.org -p 2220 /bin/sh
  ls
  cat readme
  ```
* **Key Concept:** Passing a direct command (`/bin/sh`) at the end of the `ssh` command—combined with `-t` to force pseudo-terminal allocation—spawns a minimal shell directly and bypasses the login initialization script (`.bashrc`).
* **Password:** `<REDACTED>`

---

## Level 19 → Level 20
* **Objective:** Use the setuid binary (`bandit20-do`) in the home directory to execute a command as `bandit20` and read `/etc/bandit_pass/bandit20`.
* **Command:**
  ```bash
  ls -la
  ./bandit20-do cat /etc/bandit_pass/bandit20
  ```
* **Key Concept:** A **setuid** (`s` in the owner execute permission slot, `-rwsr-x---`) binary executes with the file owner's privileges (`bandit20`) rather than the calling user's (`bandit19`), enabling access to files owned by `bandit20`.
* **Password:** `<REDACTED>`

---

## Level 20 → Level 21
* **Objective:** Set up a local Netcat listener serving the current level's password, then run the setuid binary (`suconnect`) to connect to that port and receive the password for `bandit21`.
* **Command:**
  ```bash
  echo "<BANDIT20_PASSWORD>" | nc -l -p 4900 &
  ./suconnect 4900
  ```
* **Key Concept:** Running `nc -l -p 4900 &` starts a listening TCP server in the background (`&`) on port `4900` that outputs the current password when `suconnect` connects to verify it.
* **Password:** `<REDACTED>`
*
