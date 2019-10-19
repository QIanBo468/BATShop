<template>
  <div class="listShop">
      <div class="routerback">
        <div class="back" @click="back"><img src="../../../static/images/icon/back-s.png" alt=""></div>
        <h3 class="xiangiqng">商品详情</h3>
      </div>
      <div class="zhanshi">
        <div class="zhanshitu">
          <!-- <img :src="this.listShop.imgsrc" alt=""> -->
          <img src="../../assets/shopimg_index/kuangji.png" alt="">
        </div>
        <div class="sponsor">{{jieshao}}</div>
        <div class="jifen">积分：
          <span>{{this.listShop.jifen}}</span></div>
        </div>
        <div class="purchase">
          <p>采购数量</p>
          <div class="purchaseNum">
            <div class="min" @click="min">-</div>
            <span class="num">{{purchase}}</span>
            <div class="add" @click="add">+</div>
          </div>
        </div>
        <div class="xiangqingtu">
          <div class="buyxq">详情描述</div>
          <div class="xqimg">
            <img :src="xqtusrc" alt="">
          </div>
        </div>
        <div class="buy" @click="indexbuy">购买</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      listShop: {},
      jieshao: '这里是介绍这里是介绍这里是介绍这里是介绍这里是介绍这里是介绍',
      purchase: 1,
      xqtusrc: require('../../assets/shopimg_index/kuangji.png')
    }
  },
  created() {
    this.listShop = this.$route.query.item
    console.log(this.listShop)
  },
  methods: {
    back () {
      this.$router.go(-1)
    },
    add () {
      this.purchase = this.purchase + 1;
      console.log(this.purchase)
    },
    min () {
      if (this.purchase === 0 ){
        return false
      } else {
        this.purchase = this.purchase -1;
      }
    },
    indexbuy () {
      const item = {
        listShop: this.listShop,
        jieshao: this.jieshao,
        purchase: this.purchase,
        xqtusrc: this.xqtusrc
      }
      console.log(item)
      this.$router.push({
        path: '/indexBuy',
        query:{
          item: item
          // listShops: listShop,
          // jieshaos: jieshao,
          // purchases: purchase,
          // xqtusrcs: xqtusrc
        }
      })
    }
  }
}
</script>

<style lang='less' scope>
  .listShop{
    width: 100%;
    height: 100%;
    box-sizing: border-box;
    padding:0 10px;
    // padding-right: 20px;
    // padding-top: 0;
    // padding-bottom: 0;
    overflow-y: auto;
    display: flex;
    flex-direction: column;
    align-items: center;
    background: #0b0c21;
    color: #ffffff;
    position: relative;
    .routerback{
      // display: flex;
      width: 100%;
      height: 40px;
      .back{
        float: left;
        height: 40px;
        display: flex;
        align-items: center;
      }
      .xiangiqng{
        text-align: center;
        line-height: 40px;
        margin: 0;
      }
    }
    .zhanshi{
      width: 100%;
      height: 45%;
      .zhanshitu{
        background: #1d1e3d;
        width: 100%;
        height: 60%;
        display: flex;
        justify-content: center;
        align-items: center;
        border-radius: 5px;
        img{
          width: 40%;
        }
      }
      .sponsor{
        margin-top: 5px;
        font-size: 14px;
        letter-spacing: 1px;
      }
      .jifen{
        color: red;
        margin-top: 10px;
        span{
          font-size: 18px;
          font-weight: bold;
        }
      }
    }
    .purchase{
      width: 100%;
      height: 40px;
      background: #1d1e3d;
      box-sizing: border-box;
      font-size: 14px;
      padding: 0 10px;
      color: #eeeeee;
      display: flex;
      justify-content: space-between;
      align-items: center;
      letter-spacing: 1px;
      .purchaseNum{
        display: flex;
        align-items: center;
        .min, .add{
          width: 20px;
          height: 20px;
          background: #dee7ff;
          color: #999999;
          font-size: 18px;
          text-align: center;
        }
        .num {
          display: inline-block;
          text-align: center;
          width: 20px;
          font-size: 14px;
          margin: 10px;
          box-sizing: border-box;
        }
      }
    }
    .xiangqingtu{
      width: 100%;
      display: flex;
      flex-direction: column;
      .buyxq{
        width: 100%;
        margin: 5px 0;
        padding-left: 10px;
        text-align: start;
        color: #ffffff;
        font-size: 14px;
      }
      .xqimg{
        width: 100%;
        border-radius: 8px;
        background: #ffffff;
        img{
          width: 100%;
        }
      }
    }
    .buy{
      position: fixed;
      bottom: 0;
      width: 100%;
      height: 40px;
      margin-top: 10px;
      background: #201cfa;
      font-size: 18px;
      text-align: center;
      line-height: 40px;
    }
  }
</style>