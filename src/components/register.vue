<template>
    <div id='registerBox'>
        <van-nav-bar
            title="注册账号"
            left-arrow
            @click-left="onClickLeft"
        />
        <div class='register'>
            <van-cell-group style="margin: 50px auto 201px" >
                <van-field
                    placeholder="请输入手机号"
                    maxlength="11"
                    name="account"
                    v-model="fromObj.account"
                    :error="errors.has('account')"
                    v-validate="'required|phones'"
                    autocomplete="off"
                >
                <template slot='left-icon'>
                    <img class='inputIcon' src='../../static/images/index/xingming 2@2x.png'/>
                </template>
                </van-field>
                 <van-field
                    placeholder="请输入验证码"
                    name="captcha"
                    v-validate="'required'"
                    v-model='fromObj.captcha'
                    :error="errors.has('captcha')"
                >
                <template slot='left-icon'>
                    <img class='inputIcon' src='../../static/images/index/yzm@2x.png'/>
                </template>
                <template slot="button">
                    <div @click='sendCode' class='code'>
                        {{fromObj.btntxt}}
                    </div>
                </template>
                <!-- <van-button slot="button" round size="small" plain type="primary" :disabled = disabled  ></van-button> -->
                </van-field>
                <van-field
                    placeholder="请输入登录密码（最少8位 数字+字母）"
                    v-model="fromObj.password"
                    name="password"
                    type="password"
                    :error="errors.has('password')"
                    v-validate="'required|password'"
                >
                <template slot='left-icon'>
                    <img class='inputIcon' src='../../static/images/index/jihuoma@2x.png'/>
                </template>
                </van-field>

                <van-field
                    placeholder="请再次输入登录密码"
                    v-model="fromObj.repassword"
                    left-icon="contact"
                    name='repassword'
                    type='password'
                    :error="errors.has('repassword')"
                    v-validate="'required|confirmed:password'"
                >
                <template slot='left-icon'>
                    <img class='inputIcon' src='../../static/images/index/jihuoma@2x.png'/>
                </template>
                </van-field>
                <van-field
                    placeholder="请输入邀请码"
                    left-icon="contact"
                    name='inviteCode'
                    v-validate="'required'"
                    :error="errors.has('inviteCode')"
                    v-model="fromObj.inviteCode"
                >
                <template slot='left-icon'>
                    <img class='inputIcon' src='../../static/images/index/yaoqingma@2x.png'/>
                </template>
                </van-field>
            </van-cell-group>
            <van-cell-group :border='false'>
                <van-button  class='btn' @click="submit">下一步</van-button>
                <div class='agreement' @click="flag = !flag">
                    <img class='imgIcon' v-if='flag' src='../../static/images/index/xuanze.png'>
                    <img class='imgIcon' v-if='!flag' src='../../static/images/index/xuanze(4).png'>
                    已阅读并同意以下协议：《BAT服务协议》
                </div>
            </van-cell-group>
        </div>
    </div>
</template>
<script>
import { Toast } from 'vant'
export default {
    data () {
        return {
            fromObj: {
                account: '',
                password: '',
                repassword: '',
                safeword: '',
                inviteCode: '',
                time: 60,
                btntxt: '发送验证码',
                disabled: false,
                captcha: ''
            },
            // obj: {
            //     captcha:''
            // },
            // time: 60,
            // btntxt: '发送验证码',
            // disabled: false,
            // captcha: '验证码',
            flag: true,
        }
    },
    created () {
        if(this.$route.query.from){
            this.fromObj.inviteCode = this.$route.query.from
            console.log(this.fromObj.inviteCode)
        }
    },
    methods : {
        onClickLeft () {
            this.$router.go(-1)
        },
        sendCode () {
            var phone = this.fromObj.account;
            if(!(/^[1][3,4,5,7,8][0-9]{9}$/.test(phone))){ 
                Toast("手机号码有误，请重填");  
                return false; 
            }
            if(this.disabled == false){
                this.$axios.fetchPost('/portal',
                {
                "source":"web",
                "version":"v1",
                "module":"Account",
                "interface":"1003",
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
            }

        },
        submit () {
            var that = this
            this.$validator.validateAll().then(function(result) {
                if(result){
                    if(that.flag){
                        Toast('请勾选并同意用户协议')
                        return
                    }
                    that.$axios.fetchPost('/portal',
                    {
                        source: "web",
                        version: "v1",
                        module: "Account",
                        interface: "1002",
                        data: that.fromObj
                    }).then(res => {
                        if (res.success) {
                            Toast('注册成功')
                            // that.$cookies.set('status', res.data.status)
                            that.$router.push('/login')
                            // that.$cookies.set('accessToken', res.data.tokenType + " " + res.data.accessToken , res.data.expiresIn)
                        }else{
                            Toast(res.message)
                        }
                    })
                }else{
                    console.log(that.errors)
                    Toast(that.errors.items[0].msg)
                }
            })
        }
    }
}
</script>
<style lang="less" scoped>
    #registerBox{
        width: 100%;
        height: 100%;
        background: #0b0c21;
    }
    .van-nav-bar{
    background: #0b0c21;
    
}
.van-nav-bar__title{
    color: #ffffff;
}
.van-icon-arrow-left:before{
    color: #ffffff;
}
/deep/.van-field--error .van-field__control, .van-field--error .van-field__control::placeholder{
    color: #DEE7FF;
}
/deep/.van-field__control{
    color: #DEE7FF;
}
.van-button--default{
    border: 1px solid #0b0c21;
}
.van-cell-group {
    background: #0b0c21;
    border: none;
}
.van-hairline--top-bottom::after, .van-hairline-unset--top-bottom::after{
    border-width: 0;
}
.van-cell{
    background: #0b0c21;
}
.van-hairline--bottom::after{
    border: none;
}
/deep/input:-webkit-autofill {
box-shadow: 0 0 0px 1000px #0b0c21 inset !important;
 -webkit-text-fill-color: white;
 border: 1px solid #0b0c21;
}
    .register{
        width: 343px;
        margin: 0 auto;
        .agreement{
            font-size: 12px;
            color: #999;
            margin-top: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            height: 20px;
            line-height: 20px;
            text-align: center;
        }
    }
    .inputIcon{
        width: 20px;
        height: 20px;
        margin-top: 2px;
    }
    .btn{
        width: 343px;
        height: 44px;
        background: red;
        margin: 0 auto;
        background-image: linear-gradient(90deg,#494efe, #0b02f8);
        // background:linear-gradient(180deg,rgba(253,89,102,1) 0%,rgba(231,17,34,1) 100%);
        border-radius: 22px;
        color: #fff;
    }
    .imgIcon{
        width: 13px;
        height: 13px;
        margin-right: 10px;
    }
    .code{
    // width:78px;
    padding: 0 6px;
    height:24px;
    box-sizing: border-box;
    border-radius:12px;
    border:1px solid #999999;
    // background: #F84D4D;
    color: #999999;
    text-align: center;
    font-size: 12px;
    // co
    }
</style>