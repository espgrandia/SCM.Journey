# svn command

* 在處理自動化時可能需要用到的命令。

---

## 設定 svn:externals

* 透過檔案方式設定svn 的 external

* Externals_List_File: 設定的檔案內容
  * 以一行表示一個external列表

* shell example:
    ``` sh
    svn propset svn:externals -F ${Externals_List_File} ${Externals_Folder}
    ```

* bat example:
    ``` bat
    svn propset svn:externals -F %Externals_List_File% %Externals_Folder%
    ```

---

## 設定 svn:ignore 屬性

* -F Ignore_List_File : 匯入 ignore 的檔案列表
  * 以一行表示一個檔案列表

* shell example:

    ``` sh
    svn propset svn:ignore -F ${Ignore_List_File}
    ```

* bat example:
    ``` bat
    svn propset svn:ignore -F %Ignore_List_File%
    ```

---

## auto add un files

* 判斷未加入svn檔案自動執行 svn ad

* shell example:

    ```sh
    svn st | grep ^? | awk '{print " --force "$2}' | xargs svn add
    ```

---

## auto delete miss file

* 判斷 miss files 自動執行 svn delete

* shell example:
    ```sh
    svn st | grep ^! | awk '{print " --force "$2}' | xargs svn rm
    ```

---

## svn commit

* svn 上傳
* Commit_Folder : 要上傳的資料夾.
* CommitLog_FileName : 上傳時的commit log file

* shell example:

    ``` sh
    svn commit ${Commit_Folder} -F ${CommitLog_FileName} --force-log
    ```

* bat example:

    ``` bat
    svn commit %Commit_Folder% -F %CommitLog_FileName% --force-log
    ```