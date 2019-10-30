<template>
  <div class="usesite">
    <div class="routerback">
      <div class="back" ><!--<img src="../../../static/images/icon/back-s.png" alt="">--></div>
      <h3 class="xiangiqng">我的收货地址</h3>
      <div class="addsite" @click="onClickRight">添加地址</div>
    </div>
    <div class="xuanze" v-for="(item, index) of order" :key="index">
      <div class="xuanzeicon" @click="xuanzhong(item)">
        <div class="xuanzeicons">
          <img :src="item.isDefault ? imgsrc1:imgsrc" alt="">
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
          <van-icon name="records" @click="edit(item.id)">编辑</van-icon>
        </div>
        <div class="del">
          <van-icon name="delete" @click="del(item.id)">删除</van-icon>
        </div>
      </div>
    </div>
    <div class="changesite" @click="sureAddress">
      确定
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {


      page:0,
      lastId:0,
      imgsrc: require('../../../static/images/index/xuanze.png'),
      imgsrc1: require('../../../static/images/index/xuanze(4).png'),
      order:[

      ],
      list: '',
      shopitem:{},
      addressid:''
    }
  },
  methods: {
    back () {
      this.$router.go(-1)
    },
    xuanzhong (item) {
      console.log(item)
      this.order.forEach(
        (order) => {
          order.isDefault = false
        }
      )
      item.isDefault = !item.isDefault
      this.addressid = item.id
      this.shopitem.address= item
    },
    edit (index) {
      this.$router.push({
        path: '/newAddress',
        query:{
          id:index
        }
      })
    },
    del (index) {
      this.$dialog
        .confirm({
          title: '',
          message: '确认删除此地址？'
        }).then(() => {
        // on confirm
        this.$axios.fetchPost('/portal', {
          interface: '2004',
          module: 'Address',
          source: 'web',
          version: 'v1',
          data: {
            id: index
          }
        })
          .then(res => {
            console.log(res)
            if (res.code != 1) {
              this.$toast(res.message);
              this.lastId = 0;
              this.page = 0;
              this.get_address()
              //this.order.splice(index,1)
            }
          })
      }).catch(() => {
        // on cancel
      });
    },
    onClickLeft () {
      this.$router.go(-1)
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
      this.page = this.page +1;
      this.$axios
        .fetchPost('/portal', {
          interface: '2000',
          module: 'Address',
          source: 'web',
          version: 'v1',
          data: {page:this.page,lastId:this.lastId}
        })
        .then(res => {
          this.order = res.data.list,
            this.page = res.data.currentPage,
            this.lastId = res.data.lastId
        })
    },
    sureAddress () {
      this.$router.push({
        path: '/indexBuy',
        query:{
          item: this.shopitem

        }
      })
    }
  },

  created () {
    this.shopitem = this.$route.query.item;
    console.log(this.shopitem);
    console.log(111);

    /*this.$axios.fetchPost('/portal',{
      source: "web",
      version: "v1",
      module: "Address",
      interface: "2000",
      data: {lastId: this.lastId, page: this.lastId}
    }).then(res =>{
      if(!res.success){
        this.page = res.data.currentPage;
        this.lastId = res.data.lastId;
        this.order = res.data.list;
      }
    })*/
    this.get_address()
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
