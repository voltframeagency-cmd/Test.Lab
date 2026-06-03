# Implementation Plan — Create Unified Creative Developer Toolbox Repository

This plan details the steps to aggregate all **36 compiled creative developer libraries and reference guides** (currently stored as markdown files in the App Data brain folder) into a single, clean Git repository. This will allow you to push them to GitHub/GitLab, share them, or keep them locally as a unified creative developer toolbox.

## User Review Required

> [!IMPORTANT]
> To ensure the repository is fully portable, we will perform the following actions:
> 1. **Relative Link Conversion**: Rewrite all absolute `file:///C:/Users/mahdi/...` links in the index and reference guides to clean relative links (e.g. `[cult_ui_creative_toolbox.md](cult_ui_creative_toolbox.md)`). This ensures links work on GitHub or any other machine.
> 2. **GitHub-Friendly Naming**: Rename the main central index `index.md` to `README.md` so it displays automatically as the homepage of your repository.
> 3. **Git Initialization**: Create a new folder `c:\Users\mahdi\Downloads\creative-developer-toolbox`, copy the assets there, initialize git, and make an initial commit.

## Open Questions

> [!NOTE]
> Once the repository is initialized locally, would you like us to help you push it to a remote Git hosting service (like GitHub)? If so, you can provide a remote Git URL (e.g., `git@github.com:username/repo.git`), and we can set it up and run the push command.

## Proposed Changes

We will execute a python migration script to copy, convert, and initialize the files.

### [NEW] [creative-developer-toolbox](creative-developer-toolbox)
Create a new directory containing:
* All **36 reference guides** copied from the brain folder.
* A transformed `README.md` serving as the repository's landing page.
* A `.gitignore` ignoring standard lockfiles or logs (if any).
* Initialized local `.git` repository with all files committed.

---

## Verification Plan

### Automated Verification
* Verify that the new folder contains all 36 `.md` files.
* Parse the generated `README.md` to ensure all links are relative and no absolute `file:///` paths remain.
* Verify `git status` to ensure all files are correctly tracked and committed.

### Manual Verification
* The user can inspect the folder `c:\Users\mahdi\Downloads\creative-developer-toolbox` and check the relative links in `README.md`.
