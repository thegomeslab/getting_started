# Setting up GitHub access on a remote server using a personal access token

These instructions explain how to clone a **private GitHub repository** from a remote server, such as a university Linux server or HPC login node.

GitHub no longer lets you use your normal GitHub password for command-line Git operations over HTTPS. Instead, when Git asks for a password, you use a **personal access token**, often abbreviated **PAT**.

Useful GitHub documentation:

- [Managing your personal access tokens](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/managing-your-personal-access-tokens)
- [Caching your GitHub credentials in Git](https://docs.github.com/en/get-started/git-basics/caching-your-github-credentials-in-git)
- [About authentication to GitHub](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/about-authentication-to-github)

---

## 1. Log in to the remote server

Open a terminal on your own computer.

Log in to the remote server with `ssh`:

```bash
ssh your_username@server_address
```

For example:

```bash
ssh jstudent@mycluster.university.edu
```

After logging in, you should see a command prompt on the remote server.

---

## 2. Check that Git is installed

Run:

```bash
git --version
```

You should see something like:

```text
git version 2.43.0
```

If you see:

```text
git: command not found
```

then Git is not installed or not available in your current environment.

On a university cluster, Git may be available through the module system. Try:

```bash
module avail git
```

If you see a Git module, load it:

```bash
module load git
```

Then try again:

```bash
git --version
```

---

## 3. Set your Git name and email

This tells Git who is making commits. It does **not** log you in to GitHub.

Run:

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@university.edu"
```

For example:

```bash
git config --global user.name "Jane Student"
git config --global user.email "jane-student@uiowa.edu"
```

Check that it worked:

```bash
git config --global --list
```

You should see entries like:

```text
user.name=Jane Student
user.email=jane-student@uiowa.edu
```

---

## 4. Create a GitHub personal access token

Do this part in a web browser on your own computer, not necessarily on the remote server.

1. Go to [GitHub](https://github.com/) and log in.
2. Click your profile picture in the upper right.
3. Click **Settings**.
4. Scroll down the left sidebar and click **Developer settings**.
5. Click **Personal access tokens**.
6. Choose **Fine-grained tokens**.
7. Click **Generate new token**.

Use settings like these:

```text
Token name:
    remote-server-git-access

Expiration:
    Choose an expiration date, for example 90 days or 1 year.

Resource owner:
    Choose the account or organization that owns the repository.

Repository access:
    Choose "Only select repositories", then select the private repository you need.

Permissions:
    Repository permissions → Contents → Read-only
```

For cloning a private repository, the important permission is usually:

```text
Contents: Read-only
```

If you also need to push changes back to GitHub, you may need:

```text
Contents: Read and write
```

For cloning only, use **read-only** access. It is safer.

Then click:

```text
Generate token
```

Copy the token immediately and store it somewhere secure, such as a password manager.

A token is like a password. Do **not** email it, post it in Slack, save it in a shared file, or paste it into a script.

---

## 5. Clone the private repository on the remote server

Go back to your terminal connected to the remote server.

Choose where you want the repository to live. For example:

```bash
mkdir -p ~/projects
cd ~/projects
```

Now clone the repository using the HTTPS URL.

The HTTPS clone URL looks like this:

```bash
git clone https://github.com/OWNER/REPOSITORY.git
```

For example:

```bash
git clone https://github.com/thegomeslab/private-repo.git
```

Git will ask for your GitHub username and password:

```text
Username for 'https://github.com':
Password for 'https://your_username@github.com':
```

Enter your GitHub username for the username.

For the password, **do not enter your normal GitHub password**. Paste the personal access token instead.

The token will not visibly appear as you paste it. That is normal. Press Enter.

If it works, you should see something like:

```text
Cloning into 'private-repo'...
remote: Enumerating objects: ...
Receiving objects: 100% ...
Resolving deltas: 100% ...
```

Then enter the repository:

```bash
cd private-repo
```

Check that it worked:

```bash
ls
git status
```

---

## 6. Avoid putting the token directly in the clone command

Do **not** do this:

```bash
git clone https://YOUR_TOKEN@github.com/OWNER/REPOSITORY.git
```

That can save the token in your shell history or logs.

Instead, use:

```bash
git clone https://github.com/OWNER/REPOSITORY.git
```

Then paste the token only when Git asks for the password.

---

## 7. Optional: cache the token so you are not asked every time

Without credential caching, Git may ask for your username and token each time you clone, pull, or push.

On many remote Linux systems, a simple temporary cache is enough.

For a 1-hour cache:

```bash
git config --global credential.helper 'cache --timeout=3600'
```

For an 8-hour cache:

```bash
git config --global credential.helper 'cache --timeout=28800'
```

Check your credential helper:

```bash
git config --global credential.helper
```

You should see something like:

```text
cache --timeout=28800
```

A more permanent option is Git Credential Manager, but it may not be installed on shared university servers.

---

## 8. Test pulling from the repository

After cloning, run:

```bash
git pull
```

If everything is set up correctly, Git should either update the repository or say:

```text
Already up to date.
```

---

## 9. Common errors and fixes

### Error: `Repository not found`

You may see:

```text
remote: Repository not found.
fatal: Authentication failed
```

Check:

1. Did you type the repository owner and name correctly?
2. Does your GitHub account have access to the private repository?
3. Did your token include access to this repository?
4. Did you select the correct **Resource owner** when making the token?

---

### Error: `Support for password authentication was removed`

This usually means you typed your normal GitHub password instead of your token.

Try again and paste the personal access token when Git asks for the password.

---

### Error: token does not work for an organization repository

Some GitHub organizations require approval or single sign-on authorization for tokens.

Go back to:

```text
GitHub → Settings → Developer settings → Personal access tokens
```

Find your token and look for any organization approval or SSO authorization options.

---

### Error: token expired

If the token expired, create a new token and try again.

This is normal if you selected an expiration date when creating the token.

---

### Error: Git keeps using the wrong old credentials

Git may have cached an old token or password.

You can temporarily disable caching:

```bash
git config --global --unset credential.helper
```

Then try cloning or pulling again:

```bash
git pull
```

Git should prompt for your username and token again.

---

## 10. Basic workflow after cloning

Once the repository is cloned, the usual workflow is:

```bash
cd ~/projects/private-repo
git pull
```

Edit files as needed.

Check what changed:

```bash
git status
```

If you are allowed to push changes:

```bash
git add filename.py
git commit -m "Describe the change"
git push
```

For pushing changes, the token may need `Contents: Read and write` instead of read-only. For cloning only, read-only is safer.

---

## 11. Security reminders

Treat your token like a password.

Do **not**:

- share it with another person
- paste it into a public issue, pull request, Slack message, or email
- save it in a repository
- put it directly into a `git clone` command
- put it inside a Python, Bash, Slurm, or SGE script

Do:

- give the token the smallest permissions needed
- limit it to only the repositories needed
- set an expiration date
- delete and recreate the token if you think it was exposed

---

## 12. Quick reference commands

Set Git identity:

```bash
git config --global user.name "Your Name"
git config --global user.email "your.email@university.edu"
```

Clone a private repository:

```bash
git clone https://github.com/OWNER/REPOSITORY.git
```

Move into the repository:

```bash
cd REPOSITORY
```

Check repository status:

```bash
git status
```

Pull the latest changes:

```bash
git pull
```

Set temporary credential caching for 8 hours:

```bash
git config --global credential.helper 'cache --timeout=28800'
```

Remove credential caching:

```bash
git config --global --unset credential.helper
```

