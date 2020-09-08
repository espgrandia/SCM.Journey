# Shell Read Yaml File

## 大綱

- [Shell Read Yaml File](#shell-read-yaml-file)
  - [大綱](#大綱)
  - [YAML 為何](#yaml-為何)
  - [YAML 型態簡述](#yaml-型態簡述)
  - [Shell 如何剖析 YAML](#shell-如何剖析-yaml)
  - [參考](#參考)

---

## YAML 為何

在做自動化腳本時，有時免不了需要剖析 yaml 的文件。

那什麼是 YAML 呢?

擷取自 Wiki 上面的說明:

YAML（/ˈjæməl/，尾音類似camel駱駝）是一個可讀性高，用來表達資料序列化的格式。

...

YAML 的意思其實是："Yet Another Markup Language"（仍是一種標記語言）[3]，但為了強調這種語言以數據做為中心，而不是以標記語言為重點，而用反向縮略語重新命名。

---

## YAML 型態簡述

這邊就先不說明 yaml 有哪些功能，只說明實際上會有用到及可能會使用的方式。

以我們使用上比較常見的為 key:value，而 value 可以是單一物件， dictionary 或 list 型態

- 以下為範例 :

  ``` yml

  # key : version， value: 1.0.0+1
  version: 1.0.0+1
  
  # key:assets，value : list [assets/, assets/images/, ...]
  assets:
    - assets/
    - assets/images/
    - assets/images/user/avatar/
    - assets/images/page-main/lobby/

  ```

---

## Shell 如何剖析 YAML

那如何從 shell 剖析 YAML 呢?

一開始是在下面有找到剖析方式

[Read YAML file from Bash script · GitHub](https://gist.github.com/pkuczynski/8665367)

使用方式如下:

**sample : parse_yaml.sh :**

  ``` shell
    #!/bin/sh
    parse_yaml() {
       local prefix=$2
       local s='[[:space:]]*' w='[a-zA-Z0-9_]*' fs=$(echo @|tr @ '\034')
       sed -ne "s|^\($s\)\($w\)$s:$s\"\(.*\)\"$s\$|\1$fs\2$fs\3|p" \
            -e "s|^\($s\)\($w\)$s:$s\(.*\)$s\$|\1$fs\2$fs\3|p"  $1 |
       awk -F$fs '{
          indent = length($1)/2;
          vname[indent] = $2;
          for (i in vname) {if (i > indent) {delete vname[i]}}
          if (length($3) > 0) {
             vn=""; for (i=0; i<indent; i++) {vn=(vn)(vname[i])("_")}
             printf("%s%s%s=\"%s\"\n", "'$prefix'",vn, $2, $3);
          }
       }'
    }
  ```

**sample : test.sh :**

  ``` shell
     # include parse_yaml function
     . parse_yaml.sh
     # read yaml file
     eval $(parse_yaml zconfig.yml "config_")
     # access yaml content
     echo $config_development_database
  ```

**sample : zconfig.yml :**

  ``` yml
     development:
     adapter: mysql2
     encoding: utf8
     database: my_database
     username: root
     password:
  ```

初步測試可以抓到對應的 key 及內容，不過若 value 為 list 型態則會失敗，
後來找到另一個善心人士從此範例在做改良，使用方式相同。

修正版本: [GitHub - jasperes/bash-yaml: Read a yaml file and create variables in bash](https://github.com/jasperes/bash-yaml)

經測試過可使用。

---

## 參考

- [YAML - 維基百科，自由的百科全書](https://zh.wikipedia.org/wiki/YAML)

- [The Official YAML Web Site](https://yaml.org/)
  > YAML 官方

- [YAML 語法 — ansible中文權威指南 1.0.1 documentation](https://chusiang.github.io/ansible-docs-translate/YAMLSyntax.html)

- [Read YAML file from Bash script · GitHub](https://gist.github.com/pkuczynski/8665367)
  > 原始 shell 剖析 yaml 出處，但有瑕疵。

- [GitHub - jasperes/bash-yaml: Read a yaml file and create variables in bash](https://github.com/jasperes/bash-yaml)
