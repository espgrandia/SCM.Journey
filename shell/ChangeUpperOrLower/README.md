# 字串改成全小寫或全大寫

在使用上若遇到需要將字串轉成全小寫或全大寫才能進行下一步的行為時，
可參考下面的方式來使用

``` shell
  echo $name | tr '[:lower:]' '[:upper:]'
```

- name : 為環境變數

- `tr '[:lower:]' '[:upper:]'` : 將字串中小寫字轉成大寫字，反之亦然。

---

## 參考

- [Changing to Uppercase or Lowercase - Shell Scripting Tips](https://www.shellscript.sh/tips/case/)
