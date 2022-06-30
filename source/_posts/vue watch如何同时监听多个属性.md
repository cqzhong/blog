---
title: vue watch如何同时监听多个属性
urlname: nufv9o
date: 2020-11-07 20:00:00 +0800
tags: [vue]
categories: [前端]
---

```javascript
<script>
export default {
  name: 'DialogCorrelationDimension',
  props: {
    showDimension: {
      require: true,
      type: Boolean,
      default: false
    },
    fileIds: {
      type: Array,
      default: () => []
    }
  },
  data() {
    return {
    }
  },
  computed: {
    isLoadEcho() {
      return this.fileIds.length === 1 && this.showDimension
    }
  },
  watch: {
    isLoadEcho: {
      handler(val) {
        if (val) {
          console.log('同时监听fileIds 和 showDimension')
        }
      },
      immediate: true
    }
  }
}
</script>
```
