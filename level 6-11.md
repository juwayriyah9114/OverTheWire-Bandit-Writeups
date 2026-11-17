## Level 6 → Level 7
* **Objective:** Locate a file anywhere on the server owned by user `bandit7`, owned by group `bandit6`, and 33 bytes in size.
* **Command:**
  ```bash
  find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
  cat /var/lib/dpkg/info/bandit7.password
  ```
* **Key Concept:**
  * Searching from the root directory (`/`) generates many "Permission denied" errors.
  * Appending `2>/dev/null` redirects standard error (`stderr`, file descriptor `2`) into the null device, hiding error noise so only valid paths display.
* **Password:** `<REDACTED>`

---

## Level 7 → Level 8
* **Objective:** Retrieve the password stored next to the word `millionth` inside `data.txt`.
* **Command:**
  ```bash
  grep "millionth" data.txt
  ```
* **Key Concept:** `grep` searches plain-text files line by line and prints any line matching the specified pattern or keyword.
* **Password:** `<REDACTED>`

---

## Level 8 → Level 9
* **Objective:** Find the only line of text in `data.txt` that occurs only once.
* **Command:**
  ```bash
  sort data.txt | uniq -u
  ```
* **Key Concept:**
  * `uniq` only detects duplicate lines if they are directly adjacent to each other.
  * Piping (`|`) the output of `sort` into `uniq -u` groups identical lines together first, then filters strictly for the single unique entry.
* **Password:** `<REDACTED>`

---

## Level 9 → Level 10
* **Objective:** Extract the password from `data.txt`, which is one of the few human-readable strings preceded by several `=` characters.
* **Command:**
  ```bash
  strings data.txt | grep "=="
  ```
* **Key Concept:** `strings` extracts readable ASCII character sequences from binary or corrupted files, allowing `grep` to filter out the matching lines.
* **Password:** `<REDACTED>`

---

## Level 10 → Level 11
* **Objective:** Decode the Base64-encoded contents of `data.txt`.
* **Command:**
  ```bash
  base64 -d data.txt
  ```
* **Key Concept:** Base64 is an encoding scheme that translates binary data into 64 printable ASCII characters (`A–Z`, `a–z`, `0–9`, `+`, `/`) for safe text-based transport. The `-d` flag decodes it back to plain text.
* **Password:** `<REDACTED>`

---

## Level 11 → Level 12
* **Objective:** Decode `data.txt`, where all lowercase and uppercase letters have been rotated by 13 positions (ROT13 cipher).
* **Command:**
  ```bash
  cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
  ```
* **Key Concept:** The `tr` (translate) utility substitutes characters from the first set (`A-Za-z`) with their corresponding shifted positions 13 places forward (`N-ZA-Mn-za-m`).
* **Password:** `<REDACTED>`
*
