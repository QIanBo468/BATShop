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
        <p class="usename">{{address.username}}</p>
        <p class="usestie">{{address.usesite}}</p>
        <p class="usestie">{{address.phone}}</p>
      </div>
      <van-icon name="arrow" color="#201cfa" size="20px"></van-icon>
    </div>
    <div class="shopbuy">
      <div class="shopbuys">
        <div class="shopimg">
          <img :src="shopitem.listShop.thumb" alt="">
      </div>
        <p class="shopjs">{{shopitem.listShop.description}}</p>
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

    <div class="buyway">
      <div class="buytitle">
      <div class="g"></div><p>选择支付方式</p>
      </div>
      <div class="way">
      <div class="batbuy" @click="way('bat')">
        <img :src=" buyway === 0 ? imgsrc1 : imgsrc" alt="">
        <span>BAT币({{credit_1}})</span>
      </div>
      <div class="jifenbuy" @click="way('jifen')">
        <img :src="buyway === 1 ? imgsrc1 : imgsrc" alt="">
        <span>积分({{credit_5}})</span>
      </div>
      </div>
    </div>

    <div class="endbuy">
      <div class="jifen">
        <p>合计:   {{payMoney}}积分</p>
      </div>
      <div class="queren"  @click="submit">提交订单</div>
    </div>
    </div>
</template>

<script>
import { log } from 'util'
import { Toast } from 'vant'
export default {
  data () {
    return {
      shopitem: {},
      imgsrc: '../../../static/images/index/xuanze.png',
      imgsrc1: '../../../static/images/index/xuanze(4).png',
      buyway: 0, // 0 为bat  1 为积分
      orderid: 1,
      payMoney:0,
      credit_1:0,
      credit_5:0,
      time : 3,
      payType:'credit_1',
      address: {
          usename: '',
          usesite: '',
          phone: '',
          addressId:'',
          num:0
    },
  }
  },
  created ()  {
    this.shopitem = this.$route.query.item
    console.log(this.shopitem)
    this.payMoney = this.shopitem.listShop.price * this.shopitem.purchase;
    this.num = this.shopitem.purchase


    this.$axios.fetchPost('/portal/SimpleShop',
      {
        source: "web",
        version: "v1",
        module: "Order",
        interface: "1000",
        // data: {page:this.page,lastId:this.lastId}
        data:{goodsId: this.shopitem.listShop.id,num:this.num}
      }).then(res => {
      console.log(res)
      if (res.success) {
        this.payMoney = res.data.amount;
        this.num = this.num;
        this.credit_1 = res.data.credit_1;
        this.credit_2 = res.data.credit_2;
        this.payMoney = res.data.amount;
      }
    })
    if(this.shopitem.address){
      this.address.username = this.shopitem.address.name;
      this.address.phone = this.shopitem.address.mobile;
      this.address.usesite = this.shopitem.address.address;
      this.addressId = this.shopitem.address.id;
    }else{
      this.$axios.fetchPost('/portal',
        {
          source: "web",
          version: "v1",
          module: "Address",
          interface: "3000",
          // data: {page:this.page,lastId:this.lastId}
          data:{id: this.orderid}
        }).then(res => {
        console.log(res)
        if(res.success){
          this.address.username = res.data.address.name;
          this.address.phone = res.data.address.mobile;
          this.address.usesite = res.data.address.address;
          this.addressId = res.data.address.id;
        }
      })
    }

  },
  methods: {

    back () {
      this.$router.go(-1)
    },
    site () {
        this.$router.push({
        path: '/indexSite',
        query:{
          item:  this.shopitem

        }
      })
    },
    //更新显示的商品价格
    changePrice(num){
      this.$axios.fetchPost('/portal/SimpleShop',
        {
          source: "web",
          version: "v1",
          module: "Order",
          interface: "1000",
          // data: {page:this.page,lastId:this.lastId}
          data:{goodsId: this.shopitem.listShop.id,num:num}
        }).then(res => {
        console.log(res)
        if(res.success){
          this.payMoney = res.data.amount;
          this.num = num
        }
      })
    },
    min () {
      if (this.shopitem.purchase === 0) {
        return false
      } else {
        this.shopitem.purchase = this.shopitem.purchase - 1;
        this.changePrice(this.shopitem.purchase);
      }
    },
    add () {
      this.shopitem.purchase = this.shopitem.purchase + 1
      this.changePrice(this.shopitem.purchase);
    },
    way (type){
      if(type == 'bat'){
        this.buyway = 0;
        this.payType = 'credit_1'
      }else{
        this.buyway = 1;
        this.payType = 'credit_5'
      }
    },
    submit () {
      console.log(this.addressId);
      this.$axios.fetchPost('/portal/SimpleShop',
        {
          source: "web",
          version: "v1",
          module: "Order",
          interface: "1001",
          // data: {page:this.page,lastId:this.lastId}
          data:{goodsId: this.shopitem.listShop.id,num:this.num,addressId: this.addressId,creditType:this.payType}
        }).then(res => {
        console.log(res)
        if(res.success){
          Toast(res.message)
          this.$router.push({
            path: '/my_Mill'
          })
        }else{
          Toast(res.message)
        }
      })
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
  overflow-y: auto;
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
.buyway{
  margin-top: 10px;
  width: 100%;
  background: #1d1c3b;
  border-radius: 6px;
  display: flex;
  flex-direction: column;
  .buytitle{
    display: flex;
    align-items: center;
    padding: 15px 0 ;
    .g{
        height: 20px;
        // line-height: 20px;
        width: 5px;
        background:linear-gradient(180deg,#494EFE 0%,#0C04F8 100%);
        margin-right: 10px;
        border-radius: 8px;
      }
      p{
        font-size: 16px;
        margin: 0;
      }
  }
  .way{
    width: 100%;
    display: flex;
    justify-content:space-around;
    font-size: 16px;
    margin-bottom: 10px;
    .batbuy , .jifenbuy{
      display: flex;
      align-items: center;
      text-align: center;
      img{
        margin-right: 3px;
      }
    }
  }
}
</style>
