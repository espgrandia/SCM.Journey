# 進位(制)轉換

在遇到進制轉換時，從 shell 的作法有以下兩種方式:

---

**方式一：** `使用$[]或$(())`

格式為：`$[base#number]` 或 `$((base#number))`，其中 `base` 為進位制，`number` 為對應進位制數。

這種方式輸入2進位制、16進位制等，但只能輸出為10進位制，如下：

> 16 進位制字母不分大小寫

範例:

``` shell
  # result => 12
  echo $[2#1100]

  # result => 12
  echo $((2#1100))

  # reslut => 255
  echo $[16#ff]

  # reslut => 64
  echo $[8#100]

```

---

**方式二 ：** `使用bc命令`

格式為：echo "obase=16 ; ibase=2 ; number"  |  bc ，其中 `obase` `代表輸出進位制，ibase` 代表輸入進位制，number表示 `ibase` 進位制對應的數字

注意：

- 若不設定 `ibase` 或 `obase` 的值時，該值以 base 10 當做預設值

- `obase` 要儘量放在 `ibase` 前，因為 `ibase` 設定後，後面的數字都是以 ibase 的進位制來換算的。

- 16進位制時其字母必須`大寫`。

範例 :

``` shell
  # result => 1111111111101110
  echo "ibase=16;obase=2;FFEE" | bc

  # result => CF
  echo "obase=16 ; ibase=2 ; 11001111"  |  bc

  # result => A7DD17
  echo "obase=16 ; 11001111"  |  bc

  # reslut => 110111
  echo "ibase=8 ; obase=2 ; 67"  |  bc

  # reslut => CF
  #           59CF
  echo "obase=16;ibase=2;11001111 ; 0101100111001111"  |  bc

```

---

## 注意

在 mac os (10.15.5) 的實測時，不論採取方式一或方式二，在轉換時會有精度問題。

目前實測結果，以 `16進位制`轉成`10進位制` 狀況來說，`16進位制`的位數最高到 13 位數，超過會有問題。

---

## 參考

- [linux中進位制轉換 - IT閱讀](https://www.itread01.com/content/1545148384.html)

- [Bash: echo: obase, ibase, bc: convert binary to decimal, convert decimal to binary · GitHub](https://gist.github.com/nick3499/ab63593ed48af97c6d505a41c1ff989d)

- [在 Linux Console 做 2 / 10 / 16 進位的轉換](http://blog.ilc.edu.tw/blog/index.php?op=printView&articleId=470941&blogId=25793)

- [bash - Hexadecimal To Decimal in Shell Script - Stack Overflow](https://stackoverflow.com/questions/13280131/hexadecimal-to-decimal-in-shell-script)
