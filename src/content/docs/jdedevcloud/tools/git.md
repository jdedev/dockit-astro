---
title: Git
description: Git
published: true
date: 2023-08-17T00:58:31.243Z
tags: 
editor: markdown
dateCreated: 2023-03-24T21:17:42.677Z
---

## User Profile

```shell
git config --global user.email "jdedev@gmail.com"
git config --global user.name "Serge Kurian"
```

## Repository Management

### Init with Main Branch

```shell
git init -b main
```

### Create New Repository

```shell
echo "# delete" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin git@github.com:jdedev/delete.git
git push -u origin main
```

---

## Branches

> _To be added_

---

## Common Git Commands

### Commit

```shell
git add . ; git commit -m "update" ; git push ; git status
```

### Restore Unstaged Changes

```shell
git restore .
```

---

## Submodules

### Add Submodule

```shell
git submodule add git@github.com:jdedev/doc_git.git
```

### Init/Pull Submodules (after clone)

```shell
git submodule update --init
```

### Update Submodules (get latest changes)

```shell
git submodule update --recursive --remote
```

### Clone and Check-out Submodules

Missing submodules are cloned.  
All submodules are checked out correctly.  
The latest changes are pulled from each submodule’s remote repository.

```shell
git submodule update --init --recursive
git submodule foreach --recursive git reset --
```

### Pull submodules

```shell
git pull --recurse-submodules
git submodule update --remote --merge
```
