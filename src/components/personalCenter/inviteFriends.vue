<template>
  <div class="content">
    <van-nav-bar title="邀请好友" left-arrow @click-left="$router.go(-1)" :border="false" />
    <div class="main">
      <div class="title">
        <h2>BAT邀请您注册及下载</h2>
        <div>
          <span>邀请人：{{info.nickname}}</span>
          <span>ID:{{info.id}}</span>
        </div>
      </div>
      <div class="con">
        <div class="top">
          <p>注册邀请码</p>
          <label>{{num}}</label>
          <button @click="copy(num)">复制</button>
        </div>
        <div class="bottom">
          <!-- <img :src="qrcode" alt /> -->
          <qrCode :qrcode="qrcode"></qrCode>
          <p>长按二维码识别</p>
          <!-- <button @click="show=true">邀请好友</button> -->
        </div>
      </div>
    </div>
    <!-- <van-popup v-model="show" position="bottom">
      <div class="popup">
        <ul class="flex">
          <li>
            <div class="box flex flex-align-center flex-pack-center">
              <img src="../../assets/img/weixin.png" alt />
            </div>
            <p>微信</p>
          </li>
          <li>
            <div class="box flex flex-align-center flex-pack-center">
              <img src="../../assets/img/pengyouquan.png" alt />
            </div>
            <p>朋友圈</p>
          </li>
          <li>
            <div class="box flex flex-align-center flex-pack-center">
              <img src="../../assets/img/QQ.png" alt />
            </div>
            <p>QQ</p>
          </li>
          <li>
            <div class="box flex flex-align-center flex-pack-center">
              <img src="../../assets/img/weibo.png" alt />
            </div>
            <p>微博</p>
          </li>
        </ul>
        <button @click="show = false">取消</button>
      </div>
    </van-popup> -->
  </div>
</template>

<script>

import qrCode from '../QRCode'

export default {
  data () {
    return {
      // show: true,
      num: '',
      qrcode: '',
      info: ''
    }
  },
  components: {
    qrCode
  },
  methods: {
    copy (num) {
      this.show = true
      this.$toast('已复制')
      const input = document.createElement('input')
      document.body.appendChild(input)
      input.setAttribute('value', num)
      input.select()
      if (document.execCommand('copy')) {
        document.execCommand('copy')
      }
      document.body.removeChild(input)
    }
  },
  created () {
    this.$axios
      .fetchPost('/portal', {
        interface: '4000',
        module: 'User',
        source: 'web',
        version: 'v1'
      })
      .then(res => {
        console.log(res)
        this.num = res.data.inviteCode
        this.qrcode = res.data.inviteUrl
        this.info = res.data
      })
  }
}
</script>

<style scoped>
@import "../../assets/css/reset.css";
.van-nav-bar {
  background: none;
}
.van-nav-bar .van-icon {
  color: #fff;
}
.van-nav-bar__title {
  color: #fff;
  max-width: 100%;
}
.content {
  width: 100%;
  min-height: 100vh;
  height: auto;
  /* background: linear-gradient(180deg, #fd5966 0%, #f42d3d 100%); */
  background: #0D0900;
  color: #fff;
  text-align: center;
}
.title h2 {
  font-size: 20px;
  line-height: 1;
  margin-top: 25px;
  margin-bottom: 10px;
}
.title span {
  font-size: 12px;
  line-height: 1;
  display: inline-block;
}
.title span:nth-child(1) {
  margin-right: 20px;
}
.con {
  width: 270px;
  height: 420px;
  /* background: url(../../assets/img/yaoqing_bg.png) no-repeat; */
  background: #fff;
  border-radius: 8px;
  background-size: 100% 100%;
  margin: 20px auto 0;
  color: #2D65FF;
  padding:0 25px;
}
.con .top {
  height: 166px;
  padding-top: 30px;
  border-bottom: 1px dashed #BEC6E4;
}
.con .top p {
  font-size: 14px;
  line-height: 1;
}
.con .top label {
  font-size: 35px;
  line-height: 1;
  font-weight: 500;
  margin-top: 7px;
  letter-spacing: 3px;
  display: block;
}
.main button {
  width: 80px;
  height: 28px;
  background: linear-gradient(180deg ,#9AB5FF 0%, #2D65FF 100%);
  border-radius: 8px;
  /* background: url(../../assets/img/small_btn.png) no-repeat; */
  background-size: 100% 100%;
  font-size: 12px;
  display: inline-block;
  color: #ffffff;
  margin-top: 10px;
}
.con .bottom {
  padding-top: 45px;
}
.con .bottom img {
  width: 110px;
  height: 110px;
}
.con .bottom p {
  font-size: 12px;
  line-height: 1;
  margin-top: 10px;
}
.popup ul {
  background: #f5f5f5;
  padding: 20px 6px;
}
.popup li {
  margin-left: 10px;
}
.popup .box {
  width: 60px;
  height: 60px;
  border: 1px dashed #999999;
}
.popup p {
  font-size: 10px;
  line-height: 1;
  color: #999999;
  margin-top: 10px;
}
.popup button {
  width: 100%;
  height: 50px;
  background: #fff;
  font-size: 18px;
  color: #333;
}
.van-hairline--top-bottom::after, .van-hairline-unset--top-bottom::after{
  border-width:0px 0px !important;
}
.van-cell:not(:last-child)::after{
  border: none;
}
.van-cell{
   color: #fff;
  background: #1D1C3B;
}

.van-cell__value{
            background: #f8f8f8;
        }
input{
  color: #f8f8f8 !important;
}

      
        .van-hairline--bottom::after{
            border: none;
        }
          .van-nav-bar{
            background: #0D0900;
        }
           .van-icon {
                color: #fff;
            }
            .van-nav-bar__title{
                color: #fff;
            }
            .van-nav-bar__text{
                color: #fff;
            }
</style>
