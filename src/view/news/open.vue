<template>
  <div id="main1">
    <div class="shouji">
      <img src="@/assets/images/1@2x.png" />
    </div>
    <div class="bottom-banner-container show-banner-undefined">
      <div class="slogan-wrapper">
        <img src="@/assets/images/fnlogo.png" class="logo" />
      </div>
      <div class="open">
        <a style="color:#fff;" v-on:click="open_or_download_app">打开</a>
      </div>
    </div>
  </div>
</template>

<script>
import config from "@/config";

import wx from "weixin-js-sdk";

import { getShareNews, getWxConfig, getcomment } from "@/api/article.js";

import { mapActions, mapGetters, mapState } from "vuex";
const {
  downAppURl,
  downWeixin,
  iphoneSchema,
  iphoneDownUrl,
  androidSchema,
  androidDownUrl
} = config;
export default {
  created(){
    this.open_or_download_app()
  },
  methods: {
    isWeixin: function() {
      // 判断是否是微信
      var ua = navigator.userAgent.toLowerCase();
      if (ua.match(/MicroMessenger/i) == "micromessenger") {
        return true;
      } else {
        return false;
      }
    },
    open_or_download_app() {
      var this_ = this;
      if (this.isWeixin()) {
        // 微信
        window.location = downWeixin;
      } else {
        // 非微信浏览器
        if (navigator.userAgent.match(/(iPhone|iPod|iPad);?/i)) {
          var loadDateTime = new Date();
          window.setTimeout(function() {
            var timeOutDateTime = new Date();
            if (loadDateTime && timeOutDateTime - loadDateTime < 1500 + 200) {
              window.location = iphoneDownUrl; // ios下载地址
            } else {
              window.close();
            }
          }, 2000); // 此处时间有待确定
          window.location = iphoneSchema;
        } else if (navigator.userAgent.match(/android/i)) {
          var loadDateTime = new Date();
          var ifrSrc = androidSchema;
          if (!ifrSrc) {
            return;
          }
          var ifr = document.createElement("iframe");
          ifr.src = ifrSrc;
          ifr.style.display = "none";
          document.body.appendChild(ifr);
          var TimeLoad = setTimeout(function() {
            document.body.removeChild(ifr);
            window.location = androidDownUrl; // android下载地址
          }, 2000);
          window.onblur = function() {
            clearTimeout(TimeLoad);
          };
        } else {
          // 非ios、非安卓
          window.location = downAppURl;
        }
      }
    }
  }
};
</script>

<style>
@import './open.css';
@import "./share.css";
</style>
