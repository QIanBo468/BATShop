<template>
  <div class="indexbuy">
    <div class="routerback">
      <div class="back" @click="back"><img src="../../../static/images/icon/back-s.png" alt=""></div>
      <h3 class="xiangiqng">填写订单</h3>
    </div>
    <div class="site" @click="site">
      <div class="siteicon">
        <van-icon name="location" color="#201cfa" size="30px"></van-icon>
      </div>
      <div class="buyuse">
        <p class="usename">{{order.usename}}</p>
        <p class="usestie">{{order.usesite}}</p>
      </div>
      <van-icon name="arrow" color="#201cfa" size="20px"></van-icon>
    </div>
    <div class="shopbuy">
      <div class="shopbuys">
        <div class="shopimg">
        <img :src="shopitem.listShop.imgsrc" alt="">
      </div>
        <p class="shopjs">{{shopitem.jieshao}}</p>
      </div>
      <div class="purchase">
        <p>采购数量</p>
        <div class="purchaseNum">
          <div class="min" @click="min">-</div>
          <span class="num">{{shopitem.purchase}}</span>
          <div class="add" @click="add">+</div>
        </div>
      </div>
    </div>
    <div class="endbuy">
      <div class="jifen">
        <p>合计:   {{shopitem.listShop.jifen * shopitem.purchase}}积分</p>
      </div>
      <div class="queren">提交订单</div>
    </div>
    </div>
</template>

<script>
export default {
  data () {
    return {
     shopitem: {},
     order: {
      usename: '张先生',
      usesite: '山东省临沂市 兰山区北京路23号 环球中心A座2001室',
      phone: 13371495332
     }
    }
  },
  created () {
    this.shopitem = this.$route.query.item
    console.log(this.shopitem)
  },
  methods: {
    back () {
      this.$router.go(-1)
    },
    site () {
        this.$router.push({
        path: '/indexSite',
        query:{
          // item: item
          // listShops: listShop,
          // jieshaos: jieshao,
          // purchases: purchase,
          // xqtusrcs: xqtusrc
        }
      })
    },
    min () {
      if (this.shopitem.purchase === 0) {
        return false
      } else {
        this.shopitem.purchase = this.shopitem.purchase - 1;
      }
    },
    add () {
      this.shopitem.purchase = this.shopitem.purchase + 1
    }
  }
}
</script>

<style lang='less' scope>
.indexbuy{
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
  .site{
    width: 100%;
    height: 15%;
    margin-top: 15px;
    box-sizing: border-box;
    background: #1d1c3b;
    border-radius: 8px;
    display: flex;
    align-items: center;
    padding: 0 15px;
    .siteicon{
      margin-right: 5px;
    }
    .buyuse{
      font-size: 14px;
      margin-right: 5px;
      .usename{
        font-weight: bold;
      }
      .usename , .usestie{
        margin: 5px 0px;
      }
    }
  }
  .shopbuy{
    width: 100%;
    height: 25%;
    margin-top: 15px;
    box-sizing: border-box;
    background: #1d1c3b;
    border-radius: 8px;
    display: flex;
    flex-direction: column;
    padding: 0 15px;
    .shopbuys{
      margin: 10px 0;
      width: 100%;
      display: flex;
      .shopimg{
        flex: 4;
        display: flex;
        align-items: center;
        justify-content: center;
        img{
          width: 70%;
        }
      }
      .shopjs{
        flex: 6;
        font-size: 14px;
        letter-spacing: 1px;
        line-height: 22px;
      }
    }
    .purchase{
      width: 100%;
      height: 40px;
      box-sizing: border-box;
      font-size: 15px;
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
          font-size: 15px;
          margin: 10px;
          box-sizing: border-box;
        }
      }
    }
  }
  .endbuy{
    position: fixed;
    bottom: 0;
    width: 100%;
    height: 40px;
    display: flex;
    .jifen{
      padding-left: 15px;
      font-size: 15px;
      display: flex;
      align-items: center;
      flex: 7;
      background: #1d1c3b;
    }
    .queren{
      flex: 3;
      text-align: center;
      line-height: 40px;
      // background: #201cfa;
      background-image: linear-gradient(90deg,#494efe, #0b02f8);
    }
  }
}

</style>