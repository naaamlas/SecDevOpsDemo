# Git Issue Workflow

Integrating GitHub issues with workflow automation.  
YouTube reference: [GitHub Issues + Workflow](https://www.youtube.com/watch?v=d3N2yeAaLkc)

---

## 1. Generate Personal Access Tokens (PAT)

- [Generate new PAT](https://github.com/settings/personal-access-tokens)  
- [Token settings](https://github.com/settings/tokens)

---

## 2. Clearing Linux Git Credentials

```bash
git credential-cache exit
git config --global --unset credential.helper
git config --global --unset credential.helper store
````

---

## 3. Push Again Using the PAT

```bash
git push -u origin main
```

* Enter your **USERNAME**, not email
* Use the token generated for authentication

---

## 4. Enable Credential Storage

```bash
git config --global credential.helper store
```

> Credentials will be stored as plain text on your machine.

---

## 5. Generating SSH Keys

```bash
ssh-keygen -t ed25519 -C "salman.thegr8@gmail.com"
```

---

## 6. Start SSH Agent and Add Key

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

---

## 7. Add Public Key to GitHub

```bash
cat ~/.ssh/id_ed25519.pub
```

Then go to GitHub:
**Settings → SSH and GPG keys → New SSH Key**

---

## 8. Change Repository to Use SSH

```bash
git remote set-url origin git@github.com:naaamlas/SecDevOpsDemo.git
```

---

## 9. Test SSH Connection

```bash
ssh -T git@github.com
git push
git pull
git fetch
```

> If all commands work, SSH is correctly configured.

---

## 10. Checking Git Configs

```bash
git remote -v
git config --global --list
git config --local --list
ssh-add -l
ssh -T git@github.com
```

---

## 11. Adding a File to `.gitignore`

```bash
touch secrets.txt
touch .gitignore
nano .gitignore
# add the line:
secrets.txt
git status
```

---

## 12. Untrack a File from Git

```bash
git rm --cached secrets.txt
git commit -m "Stop tracking secrets.txt"
```

---

## 13. Example `.gitignore` File

```text
config.local.json
cache/
```

---

## 14. Associating Commits with Issues

Link commits to issues without closing:

```bash
git commit -m "Refactored cache logic. see #12"
git commit -m "Fix applied for validation bug. relates to #12"
```

---

## 15. Closing the Issue via Commit

```bash
git commit -m "Fixed login bug. closes #12"
```
## 15.1 Closing the Issue via Commit

```bash
git commit -m "Fixed login and cache bugs. closes #12, closes #15"
```


Other keywords supported:

* `closes #12`
* `close #12`
* `fix #12`
* `fixes #12`
* `resolve #12`
* `resolves #12`

> GitHub will close the issue automatically when the commit is merged.

---

## 16. Adding an Issue Using cURL or GitHub CLI

* **cURL:** Use your PAT for authentication
* **GitHub CLI:** Recommended for secure workflow

---

## 17. Adding More Details in Commit Messages

```bash
git commit -m "Fix typo in login flow. closes #3

Corrected the validation regex to handle edge cases."
```

> The entire commit message will appear in the issue’s closing comment.

```