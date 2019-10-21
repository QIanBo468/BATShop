<template>
<div class="root">
  <van-nav-bar title="修改昵称" left-arrow class="nav-top" @click-left="back" />
<div class="nametitle">
      <div class="g"></div>
      <span>昵称 </span>
    </div>
  <van-cell-group>
    
    <van-field
      v-model="nickname"
      clearable
      label="姓名"
      label-class="label"
      label-width="60px"
      placeholder="请输入姓名"
      class="name"
      @blur="save"
    />
  </van-cell-group>

</div>
</template>

<script>
import { Toast } from 'vant'
export default {
  data() {
    return {
      nickname: this.$route.query.nickname,
    }
  },

  methods: {
    back() {
      this.$router.go(-1)
    },

    save() {
      this.$axios.fetchPost('/portal', {
        source: "web",
        version: "v1",
        module: "User",
        interface: "1001",
        data: {
          nickname: this.nickname,
        }
      }).then(res => {
        Toast(res.message)
        // console.log(res)
      })
    },
  },
}
</script>

<style lang="less" scoped>
.van-cell-group{
  background: #1D1C3B;
}
.van-cell{
  background: transparent;

}

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
            background: #000;
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
}
.nametitle{
  color: #f8f8f8;
  margin: 10px 20px;
  display: flex;
  align-items: center;
}
.g{
    height: 20px;
    // line-height: 20px;
    width: 5px;
    background:linear-gradient(180deg,#494EFE 0%,#0C04F8 100%);
    margin-right: 10px;
    border-radius: 8px;
  }
.nav-top {
  margin-bottom: 10px;
}

/deep/ .label {
  color: #f8f8f8;
}

.name {
  height: 60px;
  display: flex;
  align-items: center;
}

</style>
