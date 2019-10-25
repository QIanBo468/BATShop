<template>
  <div class="listShop">
      <div class="routerback">
        <div class="back" @click="back"><img src="../../../static/images/icon/back-s.png" alt=""></div>
        <h3 class="xiangiqng">商品详情</h3>
      </div>
      <div class="zhanshi">
        <div class="zhanshitu">

          <van-swipe :autoplay="3000" style="width: 100%">
            <van-swipe-item v-for="(image, index) in images" :key="index">
              <img v-lazy="image" style="width: 100%;height: 100%" />
            </van-swipe-item>
          </van-swipe>

        </div>
        <div class="sponsor">{{this.listShop.title}}</div>
        <div class="jifen">积分：
          <span>{{this.listShop.price}}</span></div>
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
          <div class="xqimg" v-html="this.listShop.detail">
            <!--<img :src="xqtusrc" alt="">-->
          </div>
        </div>
        <div class="clf"></div>
        <div class="buy" @click="indexbuy">购买</div>
  </div>
</template>

<script>
  import Vue from 'vue';
  import { Swipe, SwipeItem } from 'vant';
  import { Lazyload } from 'vant';
  Vue.use(Swipe).use(SwipeItem);
  Vue.use(Lazyload);
export default {
  data() {
    return {
      listShop: {},
      orderShop:{},
      images: [

      ],
      purchase: 1,
    }
  },
  created() {
    this.orderShop = this.$route.query.item
    this.shopid = this.$route.query.item.id
    console.log(this.orderShop)

    this.$axios.fetchPost('/portal/SimpleShop',
      {
        source: "web",
        version: "v1",
        module: "Goods",
        interface: "1001",
        data: {id:this.shopid}
      }).then(res => {
      if(res.success){
        this.listShop = res.data
        this.images = res.data.slides
      }
    })

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
        listShop: this.orderShop,
        purchase: this.purchase,
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
      height: 60px;
      position: relative;
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
        font-size: 16px;
      }
    }
    .zhanshi{
      width: 100%;
      height: 35%;
      .zhanshitu{
        background: #1d1e3d;
        width: 100%;
        height: 80%;
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
      margin: 20px;
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
        //background: #ffffff;
        img{
          width: 100%;
        }
      }
    }
    .clf{
      width: 100%;
      height: 40px;;
    }
    .buy{
      position: fixed;
      bottom: 0;
      width: 100%;
      height: 40px;
      margin-top: 10px;
      // background: #201cfa;
      background:linear-gradient(90deg,#494EFE 0%,#0C04F8 100%);
      font-size: 17px;
      text-align: center;
      line-height: 40px;
    }
  }
</style>
