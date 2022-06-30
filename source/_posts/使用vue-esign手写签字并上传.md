---
title: 使用vue-esign手写签字并上传
urlname: ie8rh7
date: 2022-01-21 20:00:00 +0800
tags: [vue-esign]
categories: [前端]
---

- 安装`vue-esign`

<!-- more -->

```bash
npm install vue-esign --save
```

- 局部使用

```vue
<!-- 员工互助 -->
<template>
  <div class="mutual-view">
    <div class="signature-view">
      <vue-esign
        ref="esign"
        :width="esignWidth"
        :height="esignHeight"
        :isCrop="isCrop"
        :lineWidth="lineWidth"
        :lineColor="lineColor"
        :bgColor.sync="bgColor"
      />
    </div>
    <div class="btns-view">
      <van-button
        color="linear-gradient(270deg, #FF5F30 0%, #FF2218 100%)"
        @click="submitSignature"
        >提交签名</van-button
      >
    </div>
  </div>
</template>

<script>
import vueEsign from "vue-esign";

import { uploadGrayFile } from "@/page/accPermissionApplication/service/getData";

export default {
  components: {
    vueEsign,
  },
  data() {
    return {
      esignWidth: window.innerWidth,
      esignHeight: window.innerHeight - 78,
      lineWidth: 6,
      lineColor: "#000000",
      bgColor: "#ffffff",
      isCrop: false,
      resultImageUrl: "",
    };
  },
  created() {
    try {
      EbeiPlugins.setNavigationRightButtons([
        {
          name: "清除",
          script: "handleReset()",
        },
      ]);
      window["handleReset"] = () => {
        this.handleReset();
      };
    } catch (e) {}
  },
  mounted() {},
  beforeDestroy() {
    try {
      EbeiPlugins.cleanNavigationRightButton();
      window["handleReset"] = null;
    } catch (e) {}
  },
  methods: {
    /**
     * 清除签名
     */
    handleReset() {
      this.$refs.esign.reset();
    },

    /**
     * 提交签名
     */
    submitSignature() {
      if (!this.$refs.esign) return;
      this.$vux.loading.show();
      this.$refs.esign
        .generate()
        .then((res) => {
          this.uploadFile(res);
        })
        .catch(() => {
          this.$vux.loading.hide();
          this.$toast("请先签名");
        })
        .finally(() => {});
    },

    /**
     * 上传图片签名
     */
    uploadFile(base64ImageData) {
      const bl = this.convertBase64UrlToBlob(base64ImageData);
      const formData = new FormData();
      formData.append("file", bl, Date.now() + ".jpeg");
      uploadGrayFile(formData)
        .then((res) => {
          if (res.status == 200 && res.data) {
            this.resultImageUrl = res.data.url;
            this.requestJoin();
            return;
          }
          this.$vux.loading.hide();
          this.$toast("签名上传失败！");
        })
        .catch(() => {
          this.$vux.loading.hide();
        });
    },

    convertBase64UrlToBlob(urlData) {
      const type = "image/jpeg";
      let bytes = null;
      if (urlData.split(",").length > 1) {
        bytes = window.atob(urlData.split(",")[1]);
      } else {
        bytes = window.atob(urlData);
      }
      let ab = new ArrayBuffer(bytes.length);
      let ia = new Uint8Array(ab);
      for (let i = 0; i < bytes.length; i++) {
        ia[i] = bytes.charCodeAt(i);
      }
      return new Blob([ab], { type });
    },
  },
};
</script>

<style lang="less" scoped>
.mutual-view {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: center;
  background-color: #ffffff;

  .signature-view {
    width: 100%;
    background-color: #ffffff;
    flex: 1;
  }

  .btns-view {
    width: 100%;
    padding: 0px 15px;
    margin-bottom: 34px;

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
