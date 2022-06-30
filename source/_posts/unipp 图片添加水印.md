---
title: unipp 图片添加水印
urlname: wg6xbl
date: 2020-05-01 20:00:00 +0800
tags: [vue,uni-app]
categories: [前端]
---

uniapp 生成水印不要使用 canvas，因为在 app 端使用 canvas 生成图片并导出时候会用 6-7 秒延迟，所以采用七牛云拼接代码。

<!-- more -->

#### 1、小程序加水印、生成海报，注意要转为 px

```html
<canvas class="wm" :style="canvasStyle" canvas-id="watermark" />
```

```javascript
import pxConfig from '../../../request/config.js'
uni.getImageInfo({
            src: tempFilePaths[0],
            success: (ress) => {
              console.log(JSON.stringify(ress, null, ' '))
              this.canvasW = ress.width
              this.canvasH = ress.height
              this.canvasStyle = `width:${this.canvasW}px; height:${this.canvasH}px;`

              const ctx = uni.createCanvasContext('watermark')
              ctx.drawImage(tempFilePaths[0], 0, 0, this.upxTopx(ress.width), this.upxTopx(ress.height))
              ctx.setFontSize(this.upxTopx(30))
              ctx.setFillStyle('blue')
              ctx.fillText('2020-03-30', 0, this.upxTopx(30))
              ctx.strokeText('2020-03-30', 0, this.upxTopx(30))
              ctx.draw(false, () => {
                this.savePoster()
              })
            }
          })

upxTopx(num) {
      return (pxConfig.SystemInfo.windowWidth / 750) * num
    },
    // 保存海报
    savePoster() {
      console.log('保存海报')
      uni.canvasToTempFilePath({
        canvasId: 'watermark',
        fileType: 'png',
        quality: 0.5,
        destWidth: this.canvasW,
        destHeight: this.canvasH,
        success: (res) => {
          this.uploadFile(res.tempFilePath, 'image')
        },
        fail(err) {
          console.log(JSON.stringify(err))
        }
      })
    }
```

```css
.wm {
  position: fixed;
  top: 500%;
  left: 0;
}
```

#### 使用七牛云图片基本处理

1、[图片基本处理（imageView2）](https://developer.qiniu.com/dora/api/1279/basic-processing-images-imageview2)
2、可视化处理方案

![](https://cdn.nlark.com/yuque/0/2020/jpeg/1028501/1596968862410-50dafcc5-21d8-4a26-a674-33cfbb20a50a.jpeg#align=left&display=inline&height=359&margin=%5Bobject%20Object%5D&originHeight=2816&originWidth=3924&size=0&status=done&style=none&width=500)

![](https://cdn.nlark.com/yuque/0/2020/jpeg/1028501/1596968860721-06c706b8-80e6-460a-b5cb-3879cc63e820.jpeg#align=left&display=inline&height=384&margin=%5Bobject%20Object%5D&originHeight=1734&originWidth=2258&size=0&status=done&style=none&width=500)
