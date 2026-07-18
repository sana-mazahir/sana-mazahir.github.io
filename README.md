# Sana Mazahir — Academic Website

This repository contains the source code for the academic website of **Sana Mazahir**, built using the [al-folio](https://github.com/alshedivat/al-folio) Jekyll theme and hosted with GitHub Pages.

- Live website: <https://sana-mazahir.github.io>
- Source repository: <https://github.com/sana-mazahir/sana-mazahir.github.io>

---

## Repository Workflow

This website uses two Git branches with different purposes.

### `main` branch — editable source code

The `main` branch contains all files that should be edited manually, including:

- Markdown pages in `_pages/`
- Homepage content in `_pages/about.md`
- Publication data in `_bibliography/papers.bib`
- CV data in `assets/json/resume.json`
- News and announcements in `_news/`
- Social links in `_data/socials.yml`
- Images in `assets/img/`
- PDF files in `assets/pdf/`
- Website configuration in `_config.yml`
- GitHub Actions deployment workflow in `.github/workflows/deploy.yml`

All normal website development must be performed on the `main` branch.

### `gh-pages` branch — generated website

The `gh-pages` branch contains the generated HTML, CSS, JavaScript, and other files served by GitHub Pages.

This branch is created and updated automatically by GitHub Actions.

**Do not manually edit, commit to, or push to the `gh-pages` branch.**

The deployment pipeline is:

```text
Edit files locally on main
          ↓
Preview with Docker Compose
          ↓
Commit and push main to GitHub
          ↓
GitHub Actions runs Jekyll
          ↓
Generated site is written to gh-pages
          ↓
GitHub Pages publishes the website
```

The generated local `_site/` directory should not be committed to `main`.

---

## Main Files to Edit

| Content | File or directory |
|---|---|
| Homepage | `_pages/about.md` |
| Publications | `_bibliography/papers.bib` |
| Web CV | `assets/json/resume.json` |
| CV-page settings | `_pages/cv.md` |
| Social links | `_data/socials.yml` |
| Announcements | `_news/` |
| Profile photograph | `assets/img/prof_pic.jpg` |
| Other images | `assets/img/` |
| Downloadable CV PDF | `assets/pdf/` |
| Website name, URL, and global settings | `_config.yml` |
| Automatic deployment workflow | `.github/workflows/deploy.yml` |

---

# Local Development with Docker Compose

Docker provides the Ruby, Jekyll, and other software needed to build the site without installing those dependencies directly on macOS.

## One-time requirements

Install and start **Docker Desktop for Mac**.

Verify that Docker is available:

```bash
docker --version
docker compose version
```

A working installation should display version numbers for both commands.

Docker Desktop must be running before using `docker compose`.

---

## Start a local editing session

### 1. Open Terminal

Navigate to the local repository:

```bash
cd ~/Documents/GitHub/sana-mazahir.github.io
```

Confirm the current location:

```bash
pwd
```

### 2. Make sure the source branch is active

```bash
git switch main
```

Confirm:

```bash
git branch --show-current
```

The output should be:

```text
main
```

### 3. Check for unfinished local changes

```bash
git status
```

Review this output before downloading changes from GitHub.

### 4. Download the latest source code

When the working tree is clean, run:

```bash
git pull --ff-only
```

This updates the local `main` branch without creating an unnecessary merge commit.

If `git status` shows uncommitted changes, review or commit them before pulling.

### 5. Start the local website

```bash
docker compose up
```

Keep this Terminal window open while editing the website.

On the first run, Docker may need to download or build an image. Later runs are usually faster.

### 6. Open the local preview

Open the following address in Safari or Chrome:

<http://localhost:8080>

This is the local preview. It is visible only on the current computer and does not change the public website.

The public website remains:

<https://sana-mazahir.github.io>

### 7. Edit the website

Open the repository in an editor such as Visual Studio Code:

```bash
code .
```

Save a file after making changes, then refresh:

<http://localhost:8080>

Most content changes are rebuilt automatically.

### 8. Restart after configuration changes

Changes to `_config.yml`, Docker files, plugins, or dependencies may require a restart.

Stop the running server by pressing:

```text
Control + C
```

Then run:

```bash
docker compose down
docker compose up
```

If Docker needs to rebuild the image, use:

```bash
docker compose up --build
```

### 9. Stop the local website

In the Terminal window running Docker, press:

```text
Control + C
```

Then cleanly stop the containers:

```bash
docker compose down
```

This does not delete the website files.

---

# Review Changes Before Committing

Always inspect the changes before sending them to GitHub.

## 1. See which files changed

```bash
git status
```

## 2. Review the changes line by line

```bash
git diff
```

Press `q` to exit the diff viewer.

For changes that have already been staged, use:

```bash
git diff --staged
```

## 3. Check the local website again

Before committing, confirm that the relevant pages work at:

```text
http://localhost:8080
```

Recommended pages to inspect include:

```text
http://localhost:8080/
http://localhost:8080/publications/
http://localhost:8080/cv/
```

Also check:

- Profile image
- Social-media icons and links
- Publication formatting
- CV sections
- Announcement dates and links
- Mobile-width layout, if possible
- Light and dark appearance, if enabled

---

# Commit and Push Changes

Perform all of these commands on the `main` branch.

## 1. Confirm the branch

```bash
git branch --show-current
```

It must display:

```text
main
```

## 2. Stage the changes

To stage all modified, added, and deleted files:

```bash
git add .
```

To stage one specific file instead:

```bash
git add path/to/file
```

For example:

```bash
git add _pages/about.md
```

## 3. Review the staged files

```bash
git status
git diff --staged
```

Confirm that no private, temporary, or unintended files are included.

## 4. Commit the changes

Use a clear message describing the update:

```bash
git commit -m "Update homepage and CV"
```

Other examples:

```bash
git commit -m "Add recent publications"
git commit -m "Update social links"
git commit -m "Add new announcement"
git commit -m "Replace profile photograph"
```

## 5. Push the source branch

```bash
git push
```

Equivalent explicit command:

```bash
git push origin main
```

This sends only the source-code commit on `main`.

Do not push directly to `gh-pages`.

---

# Automatic Deployment

A push to `main` automatically starts the GitHub Actions workflow defined in:

```text
.github/workflows/deploy.yml
```

The workflow:

1. Checks out the `main` branch.
2. Sets up Python and required Python packages.
3. Installs ImageMagick.
4. Sets up Ruby and Bundler.
5. Builds the website with Jekyll.
6. Places the generated website in `_site/`.
7. Publishes the generated files to the `gh-pages` branch.
8. GitHub Pages serves the contents of `gh-pages`.

The source code remains on `main`; generated deployment files remain on `gh-pages`.

---

# Check the Deployment

After pushing:

1. Open the GitHub repository.
2. Click **Actions**.
3. Open the newest **Deploy al-folio website** workflow run.
4. Confirm that every step has a green check mark.
5. Open <https://sana-mazahir.github.io> and verify the changes.

If the workflow fails:

1. Open the failed workflow run.
2. Click the failed job.
3. Expand the first step marked with a red X.
4. Read the final error lines.
5. Fix the source or deployment configuration on `main`.
6. Commit and push the correction.

Do not manually repair the generated `gh-pages` branch.

---

# Everyday Update Checklist

Use this sequence whenever updating the website:

```bash
cd ~/Documents/GitHub/sana-mazahir.github.io
git switch main
git status
git pull --ff-only
docker compose up
```

Then:

1. Open <http://localhost:8080>.
2. Edit the desired source files.
3. Save and refresh the local preview.
4. Check all affected pages.
5. Stop Docker with `Control + C`.
6. Run `docker compose down`.

Review and publish:

```bash
git status
git diff
git add .
git diff --staged
git commit -m "Describe the website update"
git push
```

Finally:

1. Check the GitHub Actions deployment.
2. Open the live website.
3. Confirm that the update is visible.

---

# Useful Git Recovery Commands

## Discard an unstaged change to one file

```bash
git restore path/to/file
```

Example:

```bash
git restore _pages/about.md
```

This permanently removes uncommitted changes in that file.

## Remove a file from the staging area

```bash
git restore --staged path/to/file
```

The file remains modified locally but is removed from the next commit.

## View recent commits

```bash
git log --oneline --decorate -10
```

## View configured remotes

```bash
git remote -v
```

The `origin` remote should point to:

```text
git@github.com:sana-mazahir/sana-mazahir.github.io.git
```

## Check the current branch and repository state

```bash
git status
git branch --show-current
```

---

# Important Rules

1. **Edit only the source files on `main`.**
2. **Preview all changes locally with Docker before pushing.**
3. **Never manually edit or push to `gh-pages`.**
4. **Never commit the generated `_site/` directory.**
5. **Review `git status`, `git diff`, and `git diff --staged` before committing.**
6. **Use meaningful commit messages.**
7. **Check GitHub Actions after every push.**
8. **Verify the live website after deployment.**

---

## Quick Reference

### Run locally

```bash
cd ~/Documents/GitHub/sana-mazahir.github.io
git switch main
git pull --ff-only
docker compose up
```

Open:

```text
http://localhost:8080
```

### Stop locally

```text
Control + C
```

```bash
docker compose down
```

### Publish changes

```bash
git status
git diff
git add .
git diff --staged
git commit -m "Describe the website update"
git push
```

### Deployment path

```text
main → GitHub Actions → Jekyll build → gh-pages → GitHub Pages
```
