# UTF8 問題

## 源由

> 在某次的 jenkins 建立 mac ios (Framework) 專案，
> 原始目的是透過 jenkins project 觸發該專案執行編譯並將結果commit 到 svn 所屬專案。
>
> 在此過程本地端的 mac 執行 OK，
> 但透過該方式會出現UTF8失敗(svn: E000022)。
>
> 在此記錄一下當初的歷程。

---

## svn commit 失敗錯誤訊息

* 一開始的錯誤訊息如下 (與 UTF 8 有關)

> execute svn commit
>
> svn: E000022: Commit failed (details follow):
>
> svn: E000022: Error normalizing log message to internal format
>
> svn: E000022: Can't convert string from native encoding to 'UTF-8':
>
> svn: E000022: ...

* 參考
  * [【已解决】svn error：“svn: Can’t convert string from ‘UTF-8’ to native encoding” + “svn: “–N”不像是URL”](https://www.crifan.com/resolved_svn_error_quotsvn_can39t_convert_string_from_39utf-839_to_native_encodingquot__quotsvnquot-n_quotis_not_like_urlquot/)

  * [解决svn: Can't convert string from 'UTF-8' to native encoding错误](http://www.111cn.net/sys/linux/60107.htm)

  * [linux - SVN Error: Can't convert string from native encoding to 'UTF-8' - Stack Overflow](https://stackoverflow.com/questions/2116718/svn-error-cant-convert-string-from-native-encoding-to-utf-8)

---

## 解決 UTF8 方式

* 透過 jenkins server 觸發的 shell 沒有設定 locale
* 處理方式為在 svn commit 之前強制設定 locale.

* shell example:

    ``` shell

    # show 目前的 locale 設定.
    echo "locale"
    locale

    # -a Lists all public locales.
    echo "locale -a"
    locale -a

    echo "========================"

    # 強制設定為Taiwan繁中.
    echo  "set export LANG=zh_TW.UTF-8"
    export LANG="zh_TW.UTF-8"

    # show 設定後的 locale 設定.
    echo "locale"
    locale

    # -a Lists all public locales.
    echo "locale -a"
    locale -a

    ```

* 以下為 sample log

    ``` shell
    ========================
    before - set locale
    locale
    LANG=
    LC_COLLATE="C"
    LC_CTYPE="C"
    LC_MESSAGES="C"
    LC_MONETARY="C"
    LC_NUMERIC="C"
    LC_TIME="C"
    LC_ALL=
    ---

    set export LANG=zh_TW.UTF-8
    After - set locale
    locale
    LANG="zh_TW.UTF-8"
    LC_COLLATE="zh_TW.UTF-8"
    LC_CTYPE="zh_TW.UTF-8"
    LC_MESSAGES="zh_TW.UTF-8"
    LC_MONETARY="zh_TW.UTF-8"
    LC_NUMERIC="zh_TW.UTF-8"
    LC_TIME="zh_TW.UTF-8"
    LC_ALL=
    ```

---

## 處理完後續

* 處理完之後，svn commit 的 UTF8 已經沒有警告了，但是出現 credentials 的問題，那就是另外一篇後續了 ><"

* 可參閱另一篇
  * [svn: Authentication fail](../Authentication/README.md)