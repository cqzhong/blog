---
title: 设置props在&lt;script setup lang=ts&gt;中
urlname: vuqual
date: 2022-02-22 20:00:00 +0800
tags: [setup]
categories: [前端]
---

- 类型声明 1（使用接口和范型方式）

<!-- more -->

```vue
<script setup lang="ts">
import { watch, withDefaults } from "vue";

interface PropsInterface {
  height: string;
  child: string | number;
  sda?: string;
  strData: string;
  msg?: string;
  labels?: string[];
  obj?: { a: number };
}
const props = withDefaults(defineProps<PropsInterface>(), {
  height: "60px",
  sda: "",
  msg: "",
  labels: () => [],
  obj: () => {
    return { a: 2 };
  },
});

console.log("props: ", props.height);

watch(
  () => props.height,
  (count, prevCount) => {
    console.log("height: ", count);
  },
  {
    immediate: true,
  }
);
</script>
```

- 类型声明 2

```vue
<script lang="ts" setup>
const props = defineProps<{
  child?: string | number;
  msg?: string;
  err: any[];
}>();
console.log(props);
</script>
```

- 运行时声明

```vue
<script setup lang="ts">
import { watch } from "vue";

const props = defineProps({
  cellItem: {
    type: Object,
    required: true,
    // validator:
    default: () => {
      return {
        name: "",
      };
    },
  },
});

watch(
  () => props.cellItem,
  (count, prevCount) => {
    console.log("height: ", count);
  },
  {
    immediate: true,
  }
);
</script>
```
