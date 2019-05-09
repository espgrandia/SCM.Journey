# Authentication 憑證問題

## 源由

> 在某次的 jenkins 建立 mac ios (Framework) 專案，
> 原始目的是透過 jenkins project 觸發該專案執行編譯並將結果commit 到 svn 所屬專案。
>
> 在此過程本地端的 mac 執行 OK，
> 但透過該方式會出現憑證失敗(svn: E215004)。
>
> 在此記錄一下當初的歷程。

---

## svn commit 失敗錯誤訊息

* 一開始的錯誤訊息如下 (與 Authentication 有關)

> execute svn commit
>
> svn: E170013: Commit failed (details follow):
>
> svn: E170013: Unable to connect to a repository at URL 'https://......'
>
> svn: E215004: No more credentials or we tried too many times.
>
> Authentication failed

---

## 未完，待續
