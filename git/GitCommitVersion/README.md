# GitCommitVersion

在佈署過程，會需要用到 git commit version 來做對應處理，主要為需要知道此次的 編譯的 git 對應版本為何。

---

目前知道的方式其一為，之後有更好作法會再調整用法

**剖析 git log 在拆解字串抓取我們要使用的方式 :**

- sample code :

  ``` shell
    prepreExported_GitHash_Full="$(git log -1 | grep commit | cut -d' '   -f2)"
    prepreExported_GitHash_Short=${prepreExported_GitHash_Full:0:8}
  ```

其中

- prepreExported_GitHash_Full: git commit hash 完整內容
  - `git log -1` : 抓取 git 最後 log 的內容，
  - `grep commit` : 擷取第一筆有 commit 內容
  - `cut -d' '   -f2` : 以空格 ' ' 做分隔，取分隔後第二筆內容

- prepreExported_GitHash_Short : 擷取前八位當做 short 的 hash code

範例:

> prepreExported_GitHash_Full : b590a89292a4726c7b526456f5c1c8ef3d08eb9e
>
> prepreExported_GitHash_Short : b590a892
