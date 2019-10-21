<template>
  <!-- <div class="my_address">
    <van-nav-bar
      title="地址本"
      left-arrow
      @click-left="onClickLeft"
      right-text="添加"
      @click-right="onClickRight"
    />

    <van-swipe-cell :on-close="onClose" v-for="(item,index) in list" :key="index">
      <div class="address_list">
        <div class="list_address">
          <span>{{item.name}}</span>
          <span>{{item.address}}</span>
        </div>
      </div>
      <template slot="right">
        <van-button square type="danger" text="立即删除" @click="deletes(item.id)" />
      </template>
    </van-swipe-cell>
  </div> -->
   <div class="usesite">
    <div class="routerback">
      <div class="back" @click="back"><img src="../../../static/images/icon/back-s.png" alt=""></div>
      <h3 class="xiangiqng">我的收货地址</h3>
      <div class="addsite" @click="onClickRight">添加地址</div>
    </div>
    <div class="xuanze" v-for="(item, index) of order" :key="index">
      <div class="xuanzeicon" @click="xuanzhong(item)">
        <div class="xuanzeicons">
          <img :src="item.xuanzhong ? imgsrc1:imgsrc" alt="">
        </div>
        <div class="xinxi">
          <div class="dizhi">{{item.usesite}}</div>
          <div class="phon">
            <p>{{item.usename}}</p>&nbsp;&nbsp;&nbsp;&nbsp;
            <p>{{item.phone}}</p>
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
  </div>
</template>

<script>
export default {
  data () {
    return {
      imgsrc: '../../../static/images/index/xuanze.png',
      imgsrc1: '../../../static/images/index/xuanze(4).png',
        order:[
        { usename: '张先生', usesite: '山东省临沂市 兰山区北京路23号 环球中心A座2001室', phone: 13371495332, xuanzhong: true},
        { usename: '张先生', usesite: '山东省临沂市 兰山区北京路23号 环球中心A座2001室', phone: 13371495332, xuanzhong: false},
        { usename: '张先生', usesite: '山东省临沂市 兰山区北京路23号 环球中心A座2001室', phone: 13371495332, xuanzhong: false},
        { usename: '张先生', usesite: '山东省临沂市 兰山区北京路23号 环球中心A座2001室', phone: 13371495332, xuanzhong: false}
      ],
      list: ''
      // param: '1'
    }
  },
  computed: {},
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
       this.$dialog
        .confirm({
          title: '',
          message: '确认删除此地址？'
        })
        .then(() =>{
          this.order.splice(index,1)
        })
      
      // this.$set(this.order, index, null)
      // console.log(this.order)
    },
    onClickLeft () {
      this.$router.go(-1)
    },
    deletes (id) {
      this.$dialog
        .confirm({
          title: '',
          message: '确认删除此地址？'
        })
        .then(() => {
          // 确定
          this.$axios.fetchPost('/portal', {
            interface: '8002',
            module: 'User',
            source: 'web',
            version: 'v1',
            data: {
              id: id
            }
          })
            .then(res => {
              console.log(res)
              if (res.code == 0) {
                this.$toast(res.message)
                this.get_address()
              }
            })
        })
        .catch(() => {
          // on cancel
        })
    },
    // clickPosition 表示关闭时点击的位置
    onClose (clickPosition, instance) {
      switch (clickPosition) {
        case 'left':
        case 'cell':
        case 'outside':
          instance.close()
          break
        case 'right':
          break
      }
    },
    onClickRight () {
      this.$router.push('newAddress')
    },
    get_address () {
      this.$axios
        .fetchPost('/portal', {
          interface: '8000',
          module: 'User',
          source: 'web',
          version: 'v1',
          data: {}
        })
        .then(res => {
          console.log('我的地址', res)
          this.list = res.data
          console.log(this.list)
        })
    }
  },
  created () {
    this.get_address()
  }
}
</script>

<style lang='less' scoped>
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
      font-size: 16px;
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
      border-bottom: .5px solid #707070;
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
}
// .my_address {
//   background: #f8f8f8;
//   height: 100vh;
// }
// .address_list {
//   padding: 15px;
//   background: #fff;
//   margin-top: 10px;
// }

// .list_address {
//   display: flex;
//   justify-content: center;
//   flex-direction: column;
// }
// .list_address > span:nth-child(1) {
//   color: #333333;
//   font-size: 15px;
// }
// .list_address > span:nth-child(2) {
//   color: #666666;
//   font-size: 13px;
//   margin-top: 5px;
// }
// .my_address >>> .van-button--normal {
//   height: 70px;
// }
// .van-button__text {
//   display: flex;
//   justify-content: center;
//   align-items: center;
// }
// .my_address >>> .van-dialog__message {
//   font-size: 15px;
//   color: #333333;
//   font-weight: 500;
// }
// .van-nav-bar__text {
//   color: #666666;
//   font-size: 15px;
// }
</style>
