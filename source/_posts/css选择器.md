---
title: css选择器
urlname: lzhih4
date: 2020-03-28 20:00:00 +0800
tags: [css]
categories: [前端]
---

#### 1、选择器

| 通用选择器     | \*{}             |
| -------------- | ---------------- |
| 类型选择器     | p {}             |
| 类选择器       | .note {}         |
| ID 选择器      | #introduction {} |
| 子元素选择器   | li>a {}          |
| 后代选择器     | p a {}           |
| 相邻兄弟选择器 | h1+p {}          |
| 普通兄弟选择器 | h1~p {}          |

<!-- more -->

```css
// 相邻兄弟选择器 +
// 当两个dialog-input-tipitem相邻时候
.dialog-input-tipitem + .dialog-input-tipitem {
  margin-top: 20px;
}

// 普通相邻兄弟选择器(通用兄弟选择器) ~
// 所有的同级兄弟  不要求严格相邻
h1 ~ p {
  color: red;
}

// 后代选择器
// 您希望只对 h1 元素中的 em 元素应用样式，可以这样写：
h1 em {
  color: red;
}

// 子元素选择器 >
// 如果您希望选择只作为 h1 元素子元素的 strong 元素，可以这样写：
h1 > strong {
  color: red;
}
```

#### 2、特性选择器

| 简单选择器 | []
匹配一种特定的特性 | p[class]
应用于所有包含 class 特性的<p>元素 |
| --- | --- | --- |
| 精确选择器 | [=]
匹配一种特定的特性,该特性具有特定的值 |
p[calss="dog"]
应用于所有 class 特性值为 dog 的<p>元素 |
| 部分选择器 | [~=]
匹配一个特定的特性,该特性值出现在以空格隔开的单词列表中 | p[calss~="dog"]
应用于特定的<p>元素,这些元素的 class 特性值是以空格隔开的单词列表,而其中一项是 dog |
| 开头选择器 | [^=]
匹配一个特定的特性,该特性的值以某个特定的字符串作为开头 | p[attr^"d"]
应用于特定的<p>元素,这些元素的某个特性的值以字母"d"开头 |
| 包含选择器 | [*=]
匹配一个特定的特性,该特性的值包含一个特定的子字符串 | p[attr*"do"]
应用于特定的<p>元素,这些元素的某个特性的值中含有"do" |
| 结尾选择器 | [$=]
匹配一个特定的特性,该特性的值以某个特定的字符串作为结尾 | p[attr$"g"]
应用于特定的<p>元素,这些元素的某个特性的值以字母"g"结尾 |

```css
// 精确选择器 [class^=item-]
[class^="item-"] {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
}
```

#### 3、/deep/深度选择器

- vue 组件中，在 style 设置为 scoped 的时候，里面在写样式对子组件是不生效的，如果想让某些样式对所以子组件都生效，可以使用 /deep/ 深度选择器。
- 父类有 deep 后子类自动也会深度选择

```css
<style lang="scss" scoped>
.mine_view {
  &/deep/ .tui-line-left::after {
    left: 40rpx !important;
  }
  & /deep/ .tui-cell-arrow::before {
    right: 40rpx;
  }
}


.wrap{
  .class1 {
    font-size:12px;
  }
  /deep/ .class2 {
    font-size:20px; // 对所有子组件生效.
    /deep/ .class3{   }  // 没有必要写多层deep 父类有deep后子类自动也会深度选择 并且这么写在firfox里会失效
  }
}
</style>
```

#### 4、块级元素、内联元素

- 有些元素在浏览器窗口中显示时总是另起一行.这些元素被称为块级元素。 eg:`<h1><p><ul><li>`等
- 有些元素在显示时总是与它的邻近元素出现在同一行内.这些元素被称为内联元素。
- `<div>`元素允许你将一组元素集中到一个块级元素内。
- `<span>`将文本和元素集中在一个内联元素中。
