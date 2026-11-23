## Level 12 → Level 13
* **Objective:** Revert a hexdump (`data.txt`) back into binary and repeatedly decompress multiple layers of `gzip`, `bzip2`, and `tar` archives inside a temporary directory.
* **Command:**
  ```bash
  mkdir /tmp/mywork && cd /tmp/mywork
  cp ~/data.txt .
  xxd -r data.txt > data
  file data
  # Repeat decompression steps based on 'file' output (renaming with appropriate extensions):
  mv data data.gz && gzip -d data.gz
  mv data data.bz2 && bzip2 -d data.bz2
  tar xf data
  cat data8.bin
  ```
* **Key Concept:**
  * `xxd -r` reverses a hex dump back into raw binary data.
  * Inspecting the magic bytes with `file` after each extraction reveals which decompression tool (`gzip -d`, `bzip2 -d`, or `tar xf`) to apply next until plain ASCII text remains.
* **Password:** `<REDACTED>`

---

## Level 13 → Level 14
* **Objective:** Use the private SSH key (`sshkey.private`) provided in the home directory to log into `bandit14` on localhost (or transfer the key locally via `scp` and connect).
* **Command:**
  ```bash
  # Direct connection from bandit13 via localhost:
  ssh -i sshkey.private bandit14@localhost -p 2220

  # Or copy the key locally using scp (-P specifies the uppercase port flag) and restrict permissions:
  scp -P 2220 bandit13@bandit.labs.overthewire.org:sshkey.private .
  chmod 600 sshkey.private
  ssh -i sshkey.private bandit14@bandit.labs.overthewire.org -p 2220
  ```
* **Key Concept:** Public-key authentication uses the `-i` (identity file) flag to present a private key instead of a password. SSH enforces strict file permissions (`chmod 600`) so other users cannot read the private key.
* **Password / Key:** `<REDACTED_PRIVATE_KEY>`

---

## Level 14 → Level 15
* **Objective:** Submit the current level's password to a network daemon listening on `localhost` port `30000` using Netcat (`nc`).
* **Command:**
  ```bash
  nc localhost 30000
  # Paste the bandit14 password and press Enter
  ```
* **Key Concept:** `nc` (Netcat) establishes raw TCP/UDP network connections, allowing direct text interaction with listening local services.
* **Password:** `<REDACTED>`

---

## Level 15 → Level 16
* **Objective:** Submit the current level's password to an SSL/TLS-encrypted service listening on `localhost` port `30001`.
* **Command:**
  ```bash
  openssl s_client -connect localhost:30001
  # Paste the bandit15 password and press Enter
  ```
* **Key Concept:** Standard `nc` sends unencrypted plain text and cannot complete a TLS handshake. `openssl s_client` wraps the TCP socket in SSL/TLS encryption so the remote service accepts the connection.
* **Password:** `<REDACTED>`

---

## Level 16 → Level 17
* **Objective:** Scan ports `31000–32000` on `localhost` to find which listening port speaks SSL/TLS, submit the current password, and save the returned OpenSSH private key for `bandit17`.
* **Command:**
  ```bash
  # Enumerate open ports and service versions in the target range
  nmap -v -sV -p 31000-32000 localhost

  # Connect to the identified SSL port (-quiet suppresses session handshake output)
  openssl s_client -connect localhost:<SSL_PORT> -quiet
  # Paste the bandit16 password and press Enter
  ```
* **Key Concept:** Combining `nmap -sV` service version detection with `openssl s_client -quiet` isolates the exact SSL-enabled daemon from echo services and outputs a clean private key block (`-----BEGIN OPENSSH PRIVATE KEY-----`).
* **Password / Key:** `<REDACTED_PRIVATE_KEY>`
*
