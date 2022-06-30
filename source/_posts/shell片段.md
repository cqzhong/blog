---
title: shell片段
urlname: us38g5
date: 2018-07-29 19:26:04 +0800
tags: [shell]
categories: [其它]
---

### 关键代码：去除 文件中的多行注释和 单行注释,空格行，并生成文件

```shell
sed '/^[ \t]*\/\*/,/.*\*\//d' ${localize_path}|sed '/^[ \t]*\/\//d'|\
sed '/^[[:space:]]*$/d' > ${tmp_file}


//因为使用了管道的问题，变量外的值接收不到外面的值。
cat ${tmp_file} | while read line;do
echo ${line}
done
```

<!-- more -->

### 写入多行文件

```shell
cat > ${localize_file_path_h} <<EOL
//
// This is a generated file, do not edit!
//
#import <Foundation/Foundation.h>
#import <UIKit/UIKit.h>
// 工程国际化
@interface NSString (${localize_name})
EOL
```

### 自动打包

```bash
#!/bin/sh
#!/bin/bash

# 跳转到脚本所在目录
pushd `dirname $0` > /dev/null
#另一种写法：cd $(dirname "$0")；``等同于$()

work_path=`pwd`
popd > /dev/null

declare -a array_environments

array_environments=('Debug' 'Test' 'AdHoc' 'Release')

read -p $'请输入打包环境：\n1.开发环境 \n2.测试环境\n3.预发布环境\n4.发布环境\n' index
echo  "你选择的打包环境是：${index}"

let arr_len="${#array_environments[@]}"

if [ ${index} -gt ${arr_len} ] || [ ${index} -lt 1 ]; then
index=1
fi

ipa_environment=${array_environments[index-1]}

echo ${ipa_environment}

# cd ${work_path}/CDPlaceholder_iphone
cd /Users/caoqingzhong/Desktop/GitLab/FT_iPhone/FT_iPhone

fastlane ${ipa_environment}
```

### 逐行读取文件内容，并允许外部访问变量

```shell
# 读取文件内容
index=0
unset TOTAL_STRINGS_KEYS
unset TOTAL_STRINGS_VALUES


while read line;do
#命令行内容
array=(${line//=/ })
if [ "${#array[@]}" -eq 2 ]; then

key=${array[0]}
value=${array[1]}
let lenKey="${#key}"-2
let lenValue="${#value}"-3
key=${key:1 :lenKey}
value=${value:1 : lenValue}


TOTAL_STRINGS_KEYS[index]=${key}
TOTAL_STRINGS_VALUES[index]=${value}

let index++

fi

done < ${tmp_file}


reorganizationFile localize_name TOTAL_STRINGS_KEYS TOTAL_STRINGS_VALUES
```

### 判断数字相等

```shell
if [ $index -eq 3 ]; then
echo "ADHOC"
else
echo "打包完成，下载地址是：https://www.pgyer.com/gmcT"
fi
```
