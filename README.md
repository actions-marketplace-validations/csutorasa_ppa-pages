# PPA Pages

[Personal Package Archives](https://launchpad.net/ubuntu/+ppas) for [Ubuntu](https://ubuntu.com/) on [Github Pages](https://pages.github.com/).

This action can be used in Github Pages, to [maintain an Ubuntu PPA](https://help.ubuntu.com/community/CreateAuthenticatedRepository).

## How to use

1. Create a GPG key to sign files in the PPA directory. [Github docs](https://docs.github.com/en/authentication/managing-commit-signature-verification/generating-a-new-gpg-key) has documentation for that.
2. Create Github Pages repository. [Github pages](https://pages.github.com/) has a documentation for that.
3. Create a directory in the repository.
4. Add the `.deb` files to the directory.
5. Create a repository secret [Github docs](https://docs.github.com/en/actions/security-for-github-actions/security-guides/using-secrets-in-github-actions#creating-secrets-for-a-repository) has documentation for that.
6. Create a workflow to generate the PPA files.

## Inputs

| Input            | Description                                           | Required |
| ---------------- | ----------------------------------------------------- | -------- |
| actor            | Github username to use for commit-and-push            | false    |
| actor-id         | Github user ID to use for commit-and-push             | false    |
| base-dir         | Path to the directory, where the PPA should be set up | true     |
| list-name        | Name of the .list file, preferably a unique name      | true     |
| private-key      | GPG private key to sign the files with                | true     |
| repository-owner | Github repository owner to use for the .list file     | true     |

## Outputs

There are no outputs.

## Example workflow

```yml
name: Build PPA

on:
  workflow_dispatch:

jobs:
  package:
    name: Update PPA
    runs-on: ubuntu-latest
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
      - name: Generate PPA
        uses: csutorasa/ppa-pages@main
        with:
          base-dir: ppa
          list-name: my_ppa
          private-key: ${{ secrets.PRIVATE_KEY }}
          repository-owner: ${{ github.repository_owner }}
```

## Example workflow with commit and push

```yml
name: Build PPA

on:
  workflow_dispatch:

jobs:
  package:
    name: Update PPA
    runs-on: ubuntu-latest
    permissions:
      contents: write
    steps:
      - name: Checkout repository
        uses: actions/checkout@v4
      - name: Generate and update PPA
        uses: csutorasa/ppa-pages@main
        with:
          actor: ${{ github.actor }}
          actor-id: ${{ github.actor_id }}
          base-dir: ppa
          list-name: my_ppa
          private-key: ${{ secrets.PRIVATE_KEY }}
          repository-owner: ${{ github.repository_owner }}
```
