## Level 6 → Level 7
* **Objective:** Locate a file anywhere on the server owned by user `bandit7`, owned by group `bandit6`, and 33 bytes in size[span_1](start_span)[span_1](end_span).
* **Command:**
  ```bash
  find / -user bandit7 -group bandit6 -size 33c 2>/dev/null
  cat /var/lib/dpkg/info/bandit7.password
  ```
* **Key Concept:**
  * Searching from the root directory (`/`) generates many "Permission denied" errors[span_2](start_span)[span_2](end_span).
  * Appending `2>/dev/null` redirects standard error (`stderr`, file descriptor `2`) into the null device, hiding error noise so only valid paths display[span_3](start_span)[span_3](end_span).
* **Password:** `<REDACTED>`

---

## Level 7 → Level 8
* **Objective:** Retrieve the password stored next to the word `millionth` inside `data.txt`[span_4](start_span)[span_4](end_span).
* **Command:**
  ```bash
  grep "millionth" data.txt
  ```
* **Key Concept:** `grep` searches plain-text files line by line and prints any line matching the specified pattern or keyword[span_5](start_span)[span_5](end_span).
* **Password:** `<REDACTED>`

---

## Level 8 → Level 9
* **Objective:** Find the only line of text in `data.txt` that occurs only once[span_6](start_span)[span_6](end_span).
* **Command:**
  ```bash
  sort data.txt | uniq -u
  ```
* **Key Concept:**
  * `uniq` only detects duplicate lines if they are directly adjacent to each other.
  * Piping (`|`) the output of `sort` into `uniq -u` groups identical lines together first, then filters strictly for the single unique entry[span_7](start_span)[span_7](end_span).
* **Password:** `<REDACTED>`

---

## Level 9 → Level 10
* **Objective:** Extract the password from `data.txt`, which is one of the few human-readable strings preceded by several `=` characters[span_8](start_span)[span_8](end_span).
* **Command:**
  ```bash
  strings data.txt | grep "=="
  ```
* **Key Concept:** `strings` extracts readable ASCII character sequences from binary or corrupted files, allowing `grep` to filter out the matching lines[span_9](start_span)[span_9](end_span).
* **Password:** `<REDACTED>`

---

## Level 10 → Level 11
* **Objective:** Decode the Base64-encoded contents of `data.txt`[span_10](start_span)[span_10](end_span).
* **Command:**
  ```bash
  base64 -d data.txt
  ```
* **Key Concept:** Base64 is an encoding scheme that translates binary data into 64 printable ASCII characters (`A–Z`, `a–z`, `0–9`, `+`, `/`) for safe text-based transport[span_11](start_span)[span_11](end_span). The `-d` flag decodes it back to plain text[span_12](start_span)[span_12](end_span).
* **Password:** `<REDACTED>`

---

## Level 11 → Level 12
* **Objective:** Decode `data.txt`, where all lowercase and uppercase letters have been rotated by 13 positions (ROT13 cipher)[span_13](start_span)[span_13](end_span).
* **Command:**
  ```bash
  cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
  ```
* **Key Concept:** The `tr` (translate) utility substitutes characters from the first set (`A-Za-z`) with their corresponding shifted positions 13 places forward (`N-ZA-Mn-za-m`)[span_14](start_span)[span_14](end_span).
* **Password:** `<REDACTED>`
*
