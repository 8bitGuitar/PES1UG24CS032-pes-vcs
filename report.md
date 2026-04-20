# PES-VCS Lab Report

## Phase 5 & 6: Analysis Questions

### Q5.1: Implementing `pes checkout <branch>`

To implement `pes checkout <branch>`, the following changes are needed:

**Files that change in `.pes/`:**
- `.pes/HEAD` is updated to contain `ref: refs/heads/<branch>`, pointing to the new branch.

**Working directory changes:**
1. Read the commit hash from `.pes/refs/heads/<branch>`.
2. Read the commit object to get its root tree hash.
3. Recursively walk the tree object, reading each blob, and write the file contents to the working directory at the correct paths.
4. Update `.pes/index` to reflect the checked-out tree so that `pes status` shows a clean state.

**What makes this complex:**
- Files present in the current branch but absent in the target branch must be deleted from the working directory.
- Files present in the target branch but absent in the current branch must be created.
- If the user has uncommitted modifications to tracked files that also differ between branches, checkout must abort to avoid data loss. Detecting this requires comparing the working directory state against both the current index and the target tree.
- Directory creation and removal adds further edge cases (e.g., removing the last file in a directory should remove the directory).
