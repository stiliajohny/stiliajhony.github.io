---
layout: post
comments: true
title : Git Submodules
categories: [ git, submodule, version control ]
author: John Stilia
---

<span class="img-container">
    <img src="/assets/git-submodule-01.png" >
</span>

---

If you ever used Git submodules you definitely have some scars, missing hair, and long hours of swearing.<br>
The true is, Git submodules can be tamed with few single commands and gitconfig additions


I'm currently using them for my own project. The basics are not intuitive:

If you are cloning a project with git submodules, you should use `git clone --recursive` or `git clone; git submodule init`<br><br>
If you just do a normal git clone the project's submodules won't be cloned.
Submodules are always pinned, there's no such thing as an unpinned submodule.<br>
If you make changes in the submodule, you need to `git add path/to/submodule; git commit`. <br>

To make life a bit easier, you could amend your gitconfig located at `~/.gitconfig` with the following.
```
 [pull]
         recurseSubmodules = true
         initSubmodules = true
 [checkout]
         recurseSubmodules = true
         initSubmodules = true
 [alias]
         up = pull --recurse-submodules
         co = checkout --recurse-submodules
```

---
## Git Submodule Cheatsheet


#### Create a submodule in a currently existing repo</summary>
 ```
 git submodule add URL PATH
 git submodule update --init # Adding --recursive here will also update the submodule's submodule  if it exists
 git commit -m 'Added new submodule NAME at PATH'
 ```

### Update the path of the submodule</summary>
 ```
 git submodule init
 git mv oldpath/submodule newpathsubmodule
 git submodule update
 ```

#### Check Submodules status
 ```
 $ git submodule status
 03608115df2071fff4eaaff1605768c275e5f81f bats (v0.4.0-6-g0360811)
 9f88b4207da750093baabc4e3f41bf68f0dd3630 t/test_helper/bats-assert (v0.2.0-5-g9f88b42)
 d0a131831c487a1f1141e76d3ab386c89642cdff t/test_helper/bats-support (v0.2.0)
 ```

#### Pull a submodule after cloning a repository</summary>
```
 # Your repository will have .gitmodules file
 cd ~/repository
 git submodule update --init # Adding --recursive here will also update the submodule's submodule  if it exists
```