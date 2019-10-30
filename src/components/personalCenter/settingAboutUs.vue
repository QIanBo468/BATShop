<template>
  <div class="root">
    <van-nav-bar title="关于我们" left-arrow class="nav-top" @click-left="back" />

    <div class="main">
     <!-- <div class="logo">
        <img src="../../../static/images/index/logo.png" alt="ofc矿机" />
        <div class="logo-text" style="color:#fff">logo</div>
      </div>-->

      <div class="licence">
        <h3 class="licence-head">用户注册协议</h3>
        <div class="xieyi" v-html="content"></div>
      </div>

      <div class="copy">copyright@med</div>
    </div>
  </div>
</template>

<script>
// TODO
// ofo矿机上面的图片

export default {
  data() {
    return {
      content:'未经本站的授权或许可，任何会员不得借用本站的名义从事任何商业活动，也不得将本站作为从事商业活动的场所、平台或其他任何形式的媒介。禁止将本站用作从事各种非法活动的场所、平台或者其他任何形式的媒介。违反者若触犯法律，一切后果自负，本站不承担任何责任。'
    };
  },
  created() {
    this.$axios.fetchPost("/portal", {
      source: "web",
      version: "v1",
      module: "Content",
      interface: "4003",
      data:{
        name:'about_us'
      }
    }).then(res=>{
      console.log(res)
      this.content = res.data.content
    })
  },
  methods: {
    back() {
      this.$router.go(-1);
    }
  }
};
</script>

<style lang="less" scoped>
/deep/.van-hairline--top-bottom::after, .van-hairline-unset--top-bottom::after{
  border-width:0px 0px !important;
}
 /deep/.van-cell__value{
            background: #1D1C3B;
            
        }
        /deep/.van-field__control{
            color: #f8f8f8;
        }
        .van-nav-bar{
            background: #0B0C21;
            // background:linear-gradient(180deg,#3FCFFE 0%,#39B2F8 100%);
            // background: #fff;
            .van-icon {
                color: #fff;
            }
            .van-nav-bar__title{
                color: #fff;
            }
            .van-nav-bar__text{
                color: #fff;
            }
        }
        .van-hairline--bottom::after{
            border: none;
        }
.root {
  background: #0B0C21;
  height: 100vh;
  display: flex;
  flex-direction: column;
}
.licence-head{
  color: #fff;
}
.xieyi{
  color: #D8D8D8;
  text-indent: 25px;
}
.nav-top {
  margin-bottom: 10px;
}

.main {
  display: flex;
  flex-direction: column;
  align-items: center;
  background: #0B0C21;
  flex-grow: 1;
  padding: 15px;
}

.logo {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  flex-grow: 1;

  &-text {
    font-size: 16px;
    color: #333;
    font-weight: 500;
    margin-top: 10px;
    font-weight: bold;
  }
}
.licence-head{
  margin: 0;
}
.licence {
  margin-top: 30px;
  margin-bottom: 50px;
  padding:12px 15px;
  color: #666;
  font-size: 12px;
  border-top: .5px solid #3C3D4D;
  border-bottom: .5px solid #3C3D4D;

  &-head {
    margin-bottom: 2px;
  }
}

.copy {
  margin-top: 30px;
  margin-bottom: 20px;
  color: #D8D8D8;
  font-size: 14px;
}
</style>
