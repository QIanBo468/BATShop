<template>
  <div class="fix_deal">
    <van-nav-bar title="修改支付密码" left-arrow @click-left="onClickLeft" />
    <div class="deal_list">

      <van-field
        label="请输入验证码"
        placeholder="请输入验证码"
        name="captcha"
        v-validate="'required'"
        v-model='fromObj.captcha'
        :error="errors.has('captcha')"
        autocomplete="off"
      >
      <template slot="button">
        <div @click='sendCode' class='code'>
          {{fromObj.btntxt}}
        </div>
      </template>
      </van-field>
      <van-field v-model="newPwd" placeholder="请输入新密码" label="请输入新密码" type="password" />
      <van-field v-model="checkPwd" placeholder="请再次输入新密码" label="确认新密码" type="password" />
    </div>
    <div class="sure">
      <button @click="fix_pass">完成</button>
    </div>
  </div>
</template>

<script>
  import { Toast } from 'vant'
export default {
  data() {
    return {
      current: "",
      newPwd: "",
      checkPwd: "",
      fromObj: {
        time: 60,
        btntxt: '发送验证码',
        disabled: true,
        captcha: ''
      },
    };
  },
  computed: {},
  methods: {
    onClickLeft() {
      this.$router.go(-1);
    },
    sendCode () {
      this.$axios.fetchPost('/portal',
        {
          "source":"web",
          "version":"v1",
          "module":"User",
          "interface":"2003",
          "data":
            {"account": this.fromObj.account,}
        }).then(res => {
        Toast(res.message)
        if(res.code == 0){
          this.fromObj.disabled = true;
          var  that = this
          var times = setInterval(function() {
            that.fromObj.time--;
            if( that.fromObj.time > 0){
              that.fromObj.btntxt = '重新获取('+ that.fromObj.time +'s)'
            }else{
              clearInterval(times)
              that.fromObj.time = 10
              that.fromObj.btntxt = "获取验证码";
              that.fromObj.disabled = false;
            }
          }, 1000);
        }
      }).catch( res => {
        Toast(res.message)
      })

    },
    fix_pass() {
      this.$axios
        .fetchPost("/portal", {
          interface: "2004",
          module: "User",
          source: "web",
          version: "v1",
          data: {
            captcha: this.fromObj.captcha,
            safeword: this.newPwd
          }
        })
        .then(res => {
          console.log("修改支付密码", res);
          this.$toast(res.message);
          this.current = '';
          this.newPwd = "";
          this.checkPwd='';
        });
    }
  },
  created() {}
};
</script>
<style scoped>

  .code{
    padding: 0 6px;
    height:24px;
    box-sizing: border-box;
    border-radius:12px;
    border:1px solid #999999;
    color: #999999;
    text-align: center;
    font-size: 12px;
  }
.fix_deal {
  background: #0D0900;
  min-height: 100vh;
  height: auto;
}
.deal_list {
  margin-top: 10px;
}
.fix_deal >>> .van-field__label {
  width: 149px;
}
.sure {
    display: flex;
   justify-content: center;
   width: 100%;
   height: 45px;
  /* position: fixed;
  bottom: 260px;
  left: 0;
  right: 0; */
}
.sure button {
  background-image: linear-gradient(90deg,#494efe, #0900F8);
  width: 90%;
  border: none;
  padding: 15px;
  color: #fff;
  font-size: 16px;
  border-radius: 8px;
  position: fixed;
  bottom: 15px;
}
.fix_deal>>>.van-field__control{
  color: #f8f8f8 !important;
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
