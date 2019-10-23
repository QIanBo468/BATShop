<template>
  <div class="usesite">
    <div class="routerback">
      <div class="back" @click="back"><img src="../../../static/images/icon/back-s.png" alt=""></div>
      <h3 class="xiangiqng">我的收货地址</h3>
      <div class="addsite">添加地址</div>
    </div>
    <div class="xuanze" v-for="(item, index) of order" :key="index">
      <div class="xuanzeicon" @click="xuanzhong(item)">
        <div class="xuanzeicons">
          <img :src="item.isDefault ? imgsrc:imgsrc1" alt="">
        </div>
        <div class="xinxi">
          <div class="dizhi">{{item.address}}</div>
          <div class="phon">
            <p>{{item.name}}</p>&nbsp;&nbsp;&nbsp;&nbsp;
            <p>{{item.mobile}}</p>
          </div>
        </div>
      </div>
      <div class="deladd">
        <div class="add">
          <van-icon name="records">编辑</van-icon>
        </div>
        <div class="del">
          <van-icon name="delete" @click="del(index)">删除</van-icon>
        </div>
      </div>
    </div>
    <div class="changesite">
      确定
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      imgsrc: '../../../static/images/index/xuanze.png',
      imgsrc1: '../../../static/images/index/xuanze(4).png',
      lastId: 0,
      page:0,
      order:[
        { name: '张先生', address: '山东省临沂市 兰山区北京路23号 环球中心A座2001室', mobile: 13371495332, isDefault: 0},
        { name: '张先生', address: '山东省临沂市 兰山区北京路23号 环球中心A座2001室', mobile: 13371495332, isDefault: 1},
        { name: '张先生', address: '山东省临沂市 兰山区北京路23号 环球中心A座2001室', mobile: 13371495332, isDefault: 1},
        { name: '张先生', address: '山东省临沂市 兰山区北京路23号 环球中心A座2001室', mobile: 13371495332, isDefault: 1}
      ]
    }
  },
  created() {
    this.$axios.fetchPost('/portal',{
      source: "web",
      version: "v1",
      module: "Address",
      interface: "2000",
      data: {lastId: this.lastId, page: this.lastId}
    }).then(res =>{
      // console.log(res)
      if(!res.success){
          this.page = res.data.currentPage;
          this.lastId = res.data.lastId;
          this.order = res.data.list;
        }
    })
  },
  methods: {
    back () {
      this.$router.go(-1)
    },
    xuanzhong (item) {
      console.log(item.xuanzhong)
      this.order.forEach(
        (list) => {
          list.xuanzhong = false
        }
      )
      item.xuanzhong = !item.xuanzhong
    },
    del (index) {
      this.order.splice(index,1)
      // this.$set(this.order, index, null)
      // console.log(this.order)
    }
  }
}
</script>

<style lang='less' scope>
.usesite{
  width: 100%;
  height: 100%;
  box-sizing: border-box;
  padding:0 10px;
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
    margin-bottom: 15px;
    display: flex;
    justify-content: space-between;
    align-items: center;
    .back{
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
  .xuanze{
    width: 100%;
    height: 15%;
    background: #1d1c3b;
    margin-top: 10px;
    border-radius: 8px;
    display: flex;
    flex-direction: column;
    font-size: 13px;
    color: #dddddd;
    .xuanzeicon{
      flex: 6.5;
      display: flex;
      align-items: center;
      border-bottom: .1px solid #999;
      .xuanzeicons{
        margin: 0 10px;
      }
      .xinxi{
        display: flex;
        flex-direction: column;
        .phon{
          display: flex;
          align-items: center;
          margin-top: 5px;
          p{
            margin: 0;
          }
        }
      }
    }
    .deladd{
      flex: 3.5;
      display: flex;
      justify-content: flex-end;
      align-items: center;
      .add ,.del{
        font-size: 12px;
        margin: 0 10px;
        color: #999999;
        text-align: center;
      }
    }
  }
  .changesite{
    position: fixed;
    bottom: 10px;
    width: 95%;
    box-sizing: border-box;
    // background: #201cfa;
    background-image: linear-gradient(90deg,#494efe, #0b02f8);
    height: 50px;
    border-radius: 8px;
    font-size: 16px;
    text-align: center;
    line-height: 50px;
  }
}
</style>