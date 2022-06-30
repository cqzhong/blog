---
title: html2canvas将页面生成图片并保存
urlname: lkp4ga
date: 2021-12-21 20:00:00 +0800
tags: [html2canvas]
categories: [前端]
---

- 安装`html2canvas`

<!-- more -->

```bash
npm i html2canvas
```

- 局部使用

```vue
<template>
  <div class="mutual-view">
    <mutual-guild-title slogan="感谢你加入" />
    <div class="mutual-center-view">
      <div ref="imageWrapper" class="center-cer-view">
        <div class="name">{{ staffName }}</div>
        <div class="cont">
          感谢您加入永升员工互助会，您的善意，就
          是那道光，温暖携手同行的路；在未来，我
          们相信，互助共济，爱会释放更大的能量。 互助共济，让爱永升！
        </div>
        <div class="committee-info">
          <span>旭辉永升服务工会委员会</span>
          <span class="info-employees">永升员工互助会</span>
        </div>
      </div>
    </div>
    <div class="btns-view">
      <van-button
        color="linear-gradient(270deg, #FF5F30 0%, #FF2218 100%)"
        @click="downLoadImage"
      >
        点击下载互助会证书
      </van-button>
    </div>
  </div>
</template>

<script>
import html2canvas from "html2canvas";
import MutualGuildTitle from "./components/mutual-guild-title";

import { appDownImg } from "@/util/appSdk";

export default {
  components: {
    MutualGuildTitle,
  },
  data() {
    return {
      isDownload: true,
      staffName: localStorage.userName,
    };
  },
  created() {},
  mounted() {},
  beforeDestroy() {},
  methods: {
    /**
     * 保存证书
     */
    downLoadImage() {
      this.$vux.loading.show();
      const imageWrapper = this.$refs.imageWrapper;
      html2canvas(imageWrapper, {
        allowTaint: false,
        useCORS: true,
        height: imageWrapper.scrollHeight,
        width: imageWrapper.clientWidth,
      })
        .then((canvas) => {
          const dataURL = canvas.toDataURL("image/png");
          appDownImg(dataURL);
          return dataURL;
        })
        .finally(() => {
          this.$vux.loading.hide();
        });
    },
  },
};
</script>

<style lang="less" scoped>
.mutual-view {
  width: 100%;
  min-height: 100vh;
  background: url("~@/assets/images/mutual/mutual_bg_cer.png") no-repeat;
  background-size: contain;
  background-position: top, center;
  padding: 29px 15px 34px !important;
  font-size: 0px;
  background-color: #f8f8f8;
  display: flex;
  align-items: center;
  flex-direction: column;

  .mutual-center-view {
    flex: 1;
    width: 100%;
    margin-top: 26px;
    overflow-y: auto;
    padding-bottom: 50px;

    .center-cer-view {
      width: 100%;
      height: 470px;
      background: url("~@/assets/images/mutual/mutual_cer_bg.png") no-repeat;
      background-size: contain;
      background-position: top, center;
      display: flex;
      align-items: center;
      flex-direction: column;
      padding: 153px 32px 0px;

      .name {
        height: 20px;
        font-size: 20px;
        line-height: 20px;
        font-weight: 500;
        color: #333333;
      }

      .cont {
        margin-top: 12px;
        font-size: 14px;
        font-weight: 400;
        color: #333333;
        line-height: 22px;
      }

      .committee-info {
        width: 100%;
        margin-top: 51px;
        font-size: 13px;
        line-height: 13px;
        font-weight: 400;
        color: #333333;
        display: flex;
        align-items: flex-end;
        flex-direction: column;

        .info-employees {
          margin-top: 7px;
        }
      }
    }
  }

  .btns-view {
    width: 100%;

    /deep/ button {
      margin: 0 !important;
      height: 44px !important;
      font-size: 16px !important;
    }

    /deep/ .van-button {
      width: 100% !important;
      border-radius: 6px !important;
      border: none;
      color: #ffffff;
    }
  }
}
</style>
```
