# Git Submodule Relative Path

---

## 不同登入方式的使用問題

在實務上，有遇到在同一個 submodule 設定時，
因為同仁的登入 git 方式不同，可能有下列兩種登入 git 方式

- 使用 username/ password

  - .gitmodules 類似如下

    ```text

    [submodule "submodules/scm.tools"]
        path = submodules/scm.tools
        url = https://[domain]/[group]/utils/scm.tools.git

    ```

- 使用 ssh

  - .gitmodules 類似如下

    ```text

    [submodule "submodules/scm.tools"]
        path = submodules/scm.tools
        url = git@[domain]:[group]/utils/scm.tools.git

    ```

但是在 git submodule 設定只能用其中一種方法，此時不同的登入方式會有衝突。

後來找到可以使用 relative path 的設定方式來解決此問題。

前提為設定 submodule 的 git 專案與被引用方式屬於同一個 git server 倉庫管理，

則可以使用 相對路徑的引用方式，

在 git submodule init 時會在本地端的 .git/config 自動帶入原來的登入方式

---

## 範例說明

如： 原始設定 git submodule 的

- url : https://[domain]/[group]/product/product_A.git

- ssh : git@[domain]:[group]/product/product_A.git

上面的寫法改成下面的方式:

- .gitmodules 類似如下

  ```text

  [submodule "submodules/scm.tools"]
      path = submodules/scm.tools
      url = ../../utils/scm.tools.git

  ```

不同的登入方式更新後的 ./git/config

- ./git/config for `url`

  ``` text
  [submodule "submodules/scm.tools"]
    active = true
    url = https://[domain]/[group]/utils/scm.tools.git

  ```

- ./git/config for `ssh`

  ``` text
  [submodule "submodules/scm.tools"]
    active = true
    url = git@[domain]:[group]/utils/scm.tools.git

  ```

則不同登入方式於此方式可獲得解決。

---

## 參考

- [Relative submodules](http://blog.tremily.us/posts/Relative_submodules/)

- [github - Multiple urls of git submodule - per remote submodules - Stack Overflow](https://stackoverflow.com/questions/32317365/multiple-urls-of-git-submodule-per-remote-submodules)
