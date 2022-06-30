---
title: vscode 自定义vue、react 开发模板
urlname: rasnbd
date: 2021-05-16 17:00:00 +0800
tags: [规范]
categories: [vue,react]
---

#### 1、创建代码片段

<!-- more -->

![snippet1.png](https://cdn.nlark.com/yuque/0/2021/png/1028501/1621157619281-6eab6c84-29a6-4f7f-9843-f3d7a297c225.png#align=left&display=inline&height=350&margin=%5Bobject%20Object%5D&name=snippet1.png&originHeight=350&originWidth=459&size=144915&status=done&style=none&width=459)![snippet2.png](https://cdn.nlark.com/yuque/0/2021/png/1028501/1621157626295-a5f5e8d7-52b4-42db-b656-18c0c057e45f.png#align=left&display=inline&height=169&margin=%5Bobject%20Object%5D&name=snippet2.png&originHeight=169&originWidth=615&size=28241&status=done&style=none&width=615)

#### 2、按照提示创建代码片段后、填写对应内容

```json
{
  "Print to console": {
    "prefix": "new-vue",
    "body": [
      "<!-- 文件注释 -->",
      "<template>",
      "\t<div class=\"\">\n\t\t$2\n\t</div>",
      "</template>",
      "",
      "<script>",
      "// import LeftCustomerCell from './left-customer-cell'",
      "",
      "export default {",
      "\tname: '',",
      "\tcomponents: {\n\t// LeftCustomerCell\n\t},",
      "\tprops: {},",
      "\tdata() {",
      "\t\treturn {}",
      "\t},",
      "\tcomputed: {},",
      "\twatch: {},",
      "\tbeforeCreate() {},",
      "\tcreated() {},",
      "\tbeforeMount() {},",
      "\tmounted() {},",
      "\tupdated() {},",
      "\tbeforeDestroy() {},",
      "\tdestroyed() {},",
      "\tactivated() {},",
      "\tmethods: {}",
      "}",
      "</script>",
      "",
      "<style lang=\"scss\" scoped>",
      "$4",
      "</style>",
      "$4"
    ],
    "description": "Log output to console"
  }
}
```

#### 3、注意事项

- 上面代码中的 "prefix": "new-vue", 就是快捷键, 打出`new-vue`并回车填入代码片段

![snippet3.png](https://cdn.nlark.com/yuque/0/2021/png/1028501/1621158491414-f20d9b8c-e72b-4eb6-b883-34344b834966.png#align=left&display=inline&height=737&margin=%5Bobject%20Object%5D&name=snippet3.png&originHeight=737&originWidth=822&size=84636&status=done&style=none&width=822)

#### 4、模版文件

- [new-vue.code-snippets.zip](https://www.yuque.com/attachments/yuque/0/2021/zip/1028501/1621158666154-383c5629-9942-481b-a437-5515029051fa.zip?_lake_card=%7B%22uid%22%3A%221621158665909-0%22%2C%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2Fzip%2F1028501%2F1621158666154-383c5629-9942-481b-a437-5515029051fa.zip%22%2C%22name%22%3A%22new-vue.code-snippets.zip%22%2C%22size%22%3A1003%2C%22type%22%3A%22application%2Fzip%22%2C%22ext%22%3A%22zip%22%2C%22progress%22%3A%7B%22percent%22%3A99%7D%2C%22status%22%3A%22done%22%2C%22percent%22%3A0%2C%22id%22%3A%22HlAFo%22%2C%22card%22%3A%22file%22%7D)
- [uni-vue.code-snippets.zip](https://www.yuque.com/attachments/yuque/0/2021/zip/1028501/1621158682022-2aae6911-7cc9-40b5-ad84-f79801ff83fd.zip?_lake_card=%7B%22uid%22%3A%221621158681788-0%22%2C%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2Fzip%2F1028501%2F1621158682022-2aae6911-7cc9-40b5-ad84-f79801ff83fd.zip%22%2C%22name%22%3A%22uni-vue.code-snippets.zip%22%2C%22size%22%3A1063%2C%22type%22%3A%22application%2Fzip%22%2C%22ext%22%3A%22zip%22%2C%22progress%22%3A%7B%22percent%22%3A99%7D%2C%22status%22%3A%22done%22%2C%22percent%22%3A0%2C%22id%22%3A%22vyfSJ%22%2C%22card%22%3A%22file%22%7D)
- [rn.code-snippets.zip](https://www.yuque.com/attachments/yuque/0/2021/zip/1028501/1621158699335-90475715-e50d-46fb-8299-219ab3f5e54b.zip?_lake_card=%7B%22uid%22%3A%221621158699105-0%22%2C%22src%22%3A%22https%3A%2F%2Fwww.yuque.com%2Fattachments%2Fyuque%2F0%2F2021%2Fzip%2F1028501%2F1621158699335-90475715-e50d-46fb-8299-219ab3f5e54b.zip%22%2C%22name%22%3A%22rn.code-snippets.zip%22%2C%22size%22%3A1110%2C%22type%22%3A%22application%2Fzip%22%2C%22ext%22%3A%22zip%22%2C%22progress%22%3A%7B%22percent%22%3A99%7D%2C%22status%22%3A%22done%22%2C%22percent%22%3A0%2C%22id%22%3A%22Jxelh%22%2C%22card%22%3A%22file%22%7D)
