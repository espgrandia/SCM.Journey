# Split 字串

有時會需要分割字串，以下為參考的連結內文稍加整理。

---

實際上會結合 `cut` command line 來分隔字串

先介紹 cut 用法

cut -d'[分隔字串]' -f[取分隔後第幾個字串]

- 範例 :

  ``` shell
    cut -d'_' -f2
  ```

- _ : 分隔字串為

- -f2 : 取分隔後第二個字串 (1 base)

**案例說明 :**

有個原始字串為 : one_two_three_four_five

分隔字串 : _

想要擷取 A 取自分隔後 第二個字串
想要擷取 B 取自分隔後 第四個字串

以下有有兩種使用方式來分隔字串

- 結合 cut 及 <<< 方式

``` shell
  A="$(cut -d'_' -f2 <<<'one_two_three_four_five')"
  B="$(cut -d'_' -f4 <<<'one_two_three_four_five')"
```

- 結合 echo 跟 pipe 再給 cut 處理

``` shell
  A="$(echo 'one_two_three_four_five' | cut -d'_' -f2)"
  B="$(echo 'one_two_three_four_five' | cut -d'_' -f4)"
```

---

## 參考

- [ksh - Split string by delimiter and get N-th element - Unix & Linux Stack Exchange](https://unix.stackexchange.com/questions/312280/split-string-by-delimiter-and-get-n-th-element)
