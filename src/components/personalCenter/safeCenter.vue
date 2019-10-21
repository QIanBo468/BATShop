<template>
  <div class="safe_center">
    <van-nav-bar title="账户与安全" left-arrow @click-left="onClickLeft" />
    <div class="fix_pass">
      <div class="password">
        <router-link class="login_pass" to="fix_login">
          <span>修改登录密码</span>
          <img src="../../assets/img/more_small.png" alt />
        </router-link>
        <router-link class="login_pass" to="fix_deal">
          <span>修改支付密码</span>
          <img src="../../assets/img/more_small.png" alt />
        </router-link>
      </div>
    </div>
    <div class="fix_pass">
      <div class="password">
        <div class="login_pass" @click="empty_huancun">
          <span>清空缓存</span>
          <div class="huancun">
            <span>{{huancun}}M</span>
            <img src="../../assets/img/more_small.png" alt />
          </div>
        </div>
        <div class="login_pass">
          <span>当前版本</span>
          <div class="huancun">
            <span>V1.0.0</span>
            <img src="../../assets/img/more_small.png" alt />
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      huancun:'',
      max:99,
      min:0
    }
  },
  computed: {},
  methods: {
    onClickLeft () {
      this.$router.go(-1)
    },
    empty_huancun () {
      this.$dialog({
        message:'成功清空'+this.huancun+'M缓存！'
      })
      .then(()=>{
        this.huancun = 0;
      })
    },
    // 获取缓存
    get_huancun(max=100,min=1){

      this.huancun =Math.floor(Math.random() * (max - min)) + min   
    }
  },
  created () {
    this.get_huancun()
  }
}
</script>
<style scoped>
.safe_center {
  background: #0D0900;
  height: 100vh;
}
.fix_pass {
  padding: 5px;
  background: #1D1C3B;
  margin-top: 10px;

}
.login_pass {
  padding: 10px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  /* border-bottom: 1px solid #f5f5f5; */
}
.login_pass:last-child {
  border: none;
}
.login_pass > span {
  color: #fff;
  font-size: 14px;
}
.login_pass > img {
  width: 9px;
  height: 14px;
}
.huancun {
  display: flex;
  align-items: center;
}
.huancun > span {
  color: #fff;
  font-size: 14px;
  margin-right: 5px;
  font-weight: 400;
}
.huancun > img {
  width: 9px;
  height: 14px;
}
.van-hairline--top-bottom::after, .van-hairline-unset--top-bottom::after{
  border-width:0px 0px !important;
}
.van-cell__value{
            background: #0D0900;
            
        }
.van-field__control{
            color: #f8f8f8;
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
