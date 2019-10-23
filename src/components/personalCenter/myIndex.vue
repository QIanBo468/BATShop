<template>
  <div class="myIndex">
    <div class="my_header">
      <van-nav-bar title="我的">
        <van-icon name="setting-o" size="20" slot="right" @click="onClickRight" />
      </van-nav-bar>
      <div class="top">
        <img :src="avatar" alt class="headpic" />

        <div class="headtext">
          <p>账号：{{info.account}}</p>
          <p>等级：{{info.level}}</p>
          <!-- <p>级别：{{info.level}}</p> -->
        </div>
         <div class="yaoqingma">
           <span class="yqm-q">邀请码</span>
           <span class="yqm-n">{{yaoqimgma}}</span>
         </div>
      </div>
    </div>
    <div class="my_list">
      <router-link class="list list-one" to="my_Mill">
        <div class="my_first">
          <img src="../../../static/images/index/shouhuo@3x(1).png" alt />
          <p>我的订单</p>
        </div>
        <img class="arrow"  src="../../../static/images/index/in@3x.png" alt />
      </router-link>

      <router-link class="list" to="my_Address">
        <div class="my_first">
          <img src="../../../static/images/index/shouhuo@3x.png" alt />
          <p>收货地址</p>
        </div>
        <img class="arrow"  src="../../../static/images/index/in@3x.png" alt />
      </router-link>

      <router-link class="list" to="my_team">
        <div class="my_first">
          <img src="../../../static/images/index/team@3x.png" alt />
          <p>我的团队</p>
        </div>
        <img class="arrow"  src="../../../static/images/index/in@3x.png" alt />
      </router-link>

      <router-link class="list" to="inviteFriends">
        <div class="my_first">
          <img src="../../../static/images/index/yaoqing@3x.png" alt />
          <p>邀请好友</p>
        </div>
        <img class="arrow"  src="../../../static/images/index/in@3x.png" alt />
      </router-link>

      <router-link class="list" to="safeCenter">
        <div class="my_first">
          <img src="../../../static/images/index/zhanhu@3x.png" alt />
          <p>账户与安全</p>
        </div>
        <img  class="arrow" src="../../../static/images/index/in@3x.png" alt />
      </router-link>

      
      <router-link class="list" to="feedbackNew">
        <div class="my_first">
          <img src="../../../static/images/index/wenti@3x.png" alt />
          <p>问题反馈</p>
        </div>
        <img class="arrow"  src="../../../static/images/index/in@3x.png" alt />
      </router-link>

        <router-link class="list" to="Login">
        <div class="my_first">
          <img src="../../../static/images/index/tuichu@3x.png" alt />
          <p>安全退出</p>
        </div>
        <img class="arrow" src="../../../static/images/index/in@3x.png" alt />
      </router-link>
      <!-- <router-link class="list" to='/mytrans'>
        <div class="my_first">
          <img src="../../assets/img/wodejiaoyi.png" alt />
          <p>我的交易</p>
        </div>
        <img src="../../assets/img/more_small.png" alt />
      </router-link> -->
      <!-- <router-link class="list" to="myPaymentMethods">
        <div class="my_first">
          <img src="../../assets/img/wodeshoukuanfangshi.png" alt />
          <p>我的收款方式</p>
        </div>
        <img src="../../assets/img/more_small.png" alt />
      </router-link> -->

    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      info: '',
      avatar: '',
      yaoqimgma: ''
    }
  },
  computed: {},
  methods: {
    onClickRight () {
      this.$router.push('setting')
    }
  },
  created () {
    this.$axios
      .fetchPost('/portal', {
        interface: '1000',
        module: 'User',
        source: 'web',
        version: 'v1',
        data: {}
      })
      .then(res => {
        console.log('个人信息', res)
        // this.goodsList = res.data.list;
        this.info = res.data
        this.avatar = res.data.avatar
        this.yaoqimgma = res.data.inviteCode
      })
  }
}
</script>
<style  scoped>
.myIndex{
  overflow-y: auto;
  background: #000;
}
.my_header {
  background: url(../../../static/images/index/bg@3x.png) no-repeat;
  background-size: 100% 100%;
  width: 100%;
  height: 160px;
}
.van-nav-bar {
  background: none;
}
.van-hairline--bottom::after {
  border-bottom-width: 0;
}
.van-nav-bar .van-icon {
  color: #ffffff !important;
}
.van-nav-bar__title {
  color: #ffffff;
}

.headpic {
  width: 60px;
  height: 60px;
  border-radius: 50%;
  overflow: hidden;
  margin-right: 10px;
  margin-top: 5px;
}
.top {
  margin-top: 27px;
  padding-left: 27px;
  display: flex;
  position: relative;
}
.yaoqingma{
  width: 70px;
  height: 40px;
  background: url(../../../static/images/index/171@3x.png) no-repeat no-repeat;
  background-size: 70px 40px;
  position: absolute;
  right: 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  color: #fff;
}
.yqm-q{
  font-size: .2rem;
  margin: 0;
}
.yqm-n{
  font-size: .8rem;
}
.headtext > p {
  margin: 0;
  text-align: left;
  color: rgba(255, 255, 255, 1);
}
.headtext > p:nth-child(1) {
  font-size: 16px;
  font-weight: 500;
}
.headtext > p:nth-child(2) {
  font-size: 13px;
  font-weight: 400;
  margin-top: 8px;
}
.headtext > p:nth-child(3) {
  font-size: 12px;
  font-weight: 400;
  margin-top: 8px;
}
.my_list {
  padding: 0 20px;
  position: relative;

}
.arrow{
  width: 24px;
  height: 24px;;
}

  .list-one{
    /* position: absolute; */
    /* top: 0; */
    margin-bottom: 15px;
    border: none;
  }
.list {
  background: #1D1C3B;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 10px;
  border-bottom: .5px solid #707070;
}
.my_first {
  display: flex;
  align-items: center;
  
}
.my_first > img {
  width: 22px;
  height: 22px;
}
.my_first > p {
  font-size: 14px;
  color: #fff;
  margin-left: 10px;
}
.list:first-child{
  border: none;
}
.list:last-child{
  border: none;
}
</style>
