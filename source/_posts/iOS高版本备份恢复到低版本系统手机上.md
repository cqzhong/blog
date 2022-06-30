---
title: iOS高版本备份恢复到低版本系统手机上
urlname: hhgbag
date: 2019-07-30 11:46:52 +0800
tags: [iOS]
categories: [移动端]
---

- 找到路径 ‎⁨Mac Launch⁩ ▸ ⁨ 用户 ⁩ ▸ 用户名 ▸ ⁨ 资源库 ⁩ ▸ ⁨Application Support⁩ ▸ ⁨MobileSync⁩ ▸ ⁨Backup⁩ 下的备份

<!-- more -->

- 找到备份内的 Info.plist 文件
- 修改 Product Version 字段下的版本信息。>=将要回复备份的手机版本号
- 保存
- 重启 iTunes 检查恢复备份
