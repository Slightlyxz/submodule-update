# GitHub Action: Creates a Commit when Submodules are Updated (Based on an [action doing the same, but creating a PR](https://github.com/releasehub-com/github-action-create-pr-parent-submodule))

This GitHub action creates a new commit against the parent repository when submodules are updated

**The end goal of this tool:** Automatically update the submodule in the parent repository.

## How to use

To use this **GitHub** Action you will need to complete the following in the submodule repository:

1. Parent repository has `.gitmodules` configured
2. Store GitHub token into secrets with permissions to write to parent repository
3. Create a new file in your repository called `.github/workflows/submodule-update.yml`
4. Copy the example workflow from below into that new file and modify as needed
5. Commit that file to a new branch
6. Observe the action working

This file should have the following code:

```yml
---
name: Submodule Updates

#############################
# Start the job on all push #
#############################
on:
  push

###############
# Set the Job #
###############
jobs:
  build:
    name: Submodule update
    runs-on: ubuntu-latest
    env:
      PARENT_REPOSITORY: 'org/example-repository'
      PARENT_BRANCH: 'main'

    steps:
      ##########################
      # Checkout the code base #
      ##########################
      - name: Checkout Code
        uses: actions/checkout@v2

      ####################################
      # Run the action against code base #
      ####################################
      - name: run action
        id: run_action
        uses: PaulRitter/github-action-create-commit-parent-submodule@v1
        with:
          github_token: ${{ secrets.RELEASE_HUB_SECRET }}
          parent_repository: ${{ env.PARENT_REPOSITORY }}
          parent_branch: ${{ env.PARENT_BRANCH}}
```
