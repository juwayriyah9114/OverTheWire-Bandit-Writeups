## Level 29 → Level 30
* **Objective:** Clone the `bandit29-git` repository, enumerate remote branches, and check out the `dev` branch to retrieve the password omitted from `master`.
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
* **Key Concept:** `git clone` checks out only the default branch (`master`) locally. Running `git branch -a` exposes hidden remote branches (`remotes/origin/dev`), which can be switched to using `git checkout dev`.
* **Password:** `jq9Dfg2rXsfYsWWgFuKlXhphjdH7USgX`

---

## Level 30 → Level 31
* **Objective:** Clone the `bandit30-git` repository and inspect Git tags to recover the hidden `bandit31` password.
* **Commands:**
  ```bash
  mkdir bandit30
  cd bandit30
  GIT_SSH_COMMAND="ssh -4 -p 2220" git clone ssh://bandit30-git@bandit.labs.overthewire.org/home/bandit30-git/repo
  cd repo
  git tag
  git show secret
  ```
* **Key Concept:** Sensitive values stored inside annotated or lightweight Git tags do not appear in standard file listings or commit logs; `git tag` lists all tags and `git show <tag>` prints the tagged object's contents.
* **Password:** `82NkymblpGBYmIXG6ZQ8YldBYstHpfUf`

---

## Level 31 → Level 32
* **Objective:** Bypass `.gitignore` rules in the `bandit31-git` repository by force-adding `key.txt` (`May I come in?`) and pushing it to `origin master` to trigger the server's pre-receive hook output.
* **Commands:**
  ```bash
  mkdir bandit31
  cd bandit31
  GIT_SSH_COMMAND="ssh -4 -p 2220" git clone ssh://bandit31-git@bandit.labs.overthewire.org/home/bandit31-git/repo
  cd repo
  echo "May I come in?" > key.txt
  git add -f key.txt
  git commit -m "add key.txt"
  git status
  GIT_SSH_COMMAND="ssh -4 -p 2220" git push origin master
  ```
* **Key Concept:** A `.gitignore` file blocks `*.txt` files from normal staging, but `git add -f` overrides ignore rules. Pushing the commit triggers a server-side Git hook that validates the file content and prints the next password before rejecting the push.
* **Password:** `pWuj5jBQ6IgVONXwiH6g1pXRF8S1YvbT`

---

## Level 32 → Level 33
* **Objective:** Escape the restricted `uppershell` environment (which converts all alphabetic input to uppercase) using positional shell parameters to spawn `/bin/sh`.
* **Commands:**
  ```bash
  $0
  cat /etc/bandit_pass/bandit33
  ```
* **Key Concept:** Because `uppershell` translates letters like `sh` or `bash` into invalid uppercase commands (`SH`, `BASH`), special shell variables that contain no letters—such as `$0` (which expands to the name of the current shell process, `/bin/sh`)—bypass uppercase filtering and drop directly into an unrestricted shell.
* **Password:** `u4P2CyPOwPGLe94RdD9Uo2FxFwvnfSmM`
*
