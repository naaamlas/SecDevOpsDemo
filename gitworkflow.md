# Git issue workflow

github issues + workflow 
https://www.youtube.com/watch?v=d3N2yeAaLkc

## 1. Generate Tokens
https://github.com/settings/personal-access-tokens
https://github.com/settings/tokens


## 2. clearing Linux Creds
git credential-cache exit
git config --global --unset credential.helper
git config --global --unset credential.helper store

## 3. Push Again Using the PAT
git push -u origin main
make sure you enter your USERNAME, not the email
Use the token generatd and push the repo

## enable Cred Storage
git config --global credential.helper store

creds stored as plain text files.


## generating ssh keys
ssh-keygen -t ed25519 -C "salman.thegr8@gmail.com"

## ssh agent start
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519

## add public key to github
cat ~/.ssh/id_ed25519.pub

Go to GitHub:
Settings → SSH and GPG keys → New SSH Key

## changing to ssh
git remote set-url origin git@github.com:naaamlas/SecDevOpsDemo.git

## test
ssh -T git@github.com
gt push
git pull
git fetch

if everything is done.. all good. 

## checking configs
git remote -v
git config --global --list
git config --local --list
ssh-add -l
ssh -T git@github.com

## adding a file to ignore.
touch secrets.txt
touch .gitignore
nano .gitignore
secrets.txt
git status

## un track a file from git
git rm --cached secrets.txt
git commit -m "Stop tracking secrets.txt"


## so your .gitignore file could look like this.
config.local.json
cache/


## Associating your commits with issues.
git commit -m "Refactored cache logic. see #12"
git commit -m "Fix applied for validation bug. relates to #12"

## closing the issue on commit.
git commit -m "Fixed login bug. closes #12"
use these keywords
closes #12
close #12
fixes #12
resolve #12
resolves #12


## adding an issue using curl and or github cli

## curl
curl -X POST \
  -H "Accept: application/vnd.github+json" \
  -H "Authorization: Bearer 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" \
  https://api.github.com/repos/naaamlas/SecDevOpsDemo/issues \
  -d '{"title":"Bug found","adding in new readme":"Details here"}'


## adding in more in comments.
	git commit -m "Fix typo in login flow. closes #3
	
	Corrected the validation regex to handle edge cases."