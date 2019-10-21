<template>
  <div class="mymill_bj">
    <van-nav-bar title="我的订单" left-arrow @click-left="onClickLeft" />
    <div>
      <van-list v-model="loading" :finished="finished" finished-text="没有更多了" @load="get_mill">
        <div class="mill_list" v-for="(item,index) in list " :key="index">
          <div class="list">
            <div class="list-img">
              <img class="list-img" :src="item.listimg" alt />
            </div>
            <div class="intrudce">
              <p>{{item.listshop}}</p>
              <div class="intrudce-p">
              <p>￥{{item.listmeny}}</p>
              <p>x{{item.listnum}}</p>
              </div>
              <!-- <div class="price">
                <p>累计收益：</p>
                <p>{{item.grantedYield}}ofc</p>
              </div> -->
            </div>
          </div>
        </div>
      </van-list>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      lastId: '',
      page: 1,
      loading: false,
      finished: false,
      list: [
        {listshop: '双面渔夫帽女夏季日系大檐帽时尚百搭防晒遮阳韩版网红ins太阳帽', listmeny: 380, listnum: 2 ,listimg:require('../../assets/shopimg_index/kuangji.png')},
         {listshop: '双面渔夫帽女夏季日系大檐帽时尚百搭防晒遮阳韩版网红ins太阳帽', listmeny: 380, listnum: 2 ,listimg:require('../../assets/shopimg_index/kuangji.png')},
          {listshop: '双面渔夫帽女夏季日系大檐帽时尚百搭防晒遮阳韩版网红ins太阳帽', listmeny: 380, listnum: 2 ,listimg:require('../../assets/shopimg_index/kuangji.png')},
           {listshop: '双面渔夫帽女夏季日系大檐帽时尚百搭防晒遮阳韩版网红ins太阳帽', listmeny: 380, listnum: 2 ,listimg:require('../../assets/shopimg_index/kuangji.png')}
      ]
    }
  },
  computed: {},
  methods: {
    onClickLeft () {
      this.$router.go(-1)
    },
    get_mill () {
      var lastid = ''
      if (this.lastId) {
        lastid = this.lastId
      } else {
        lastid = 0
      }
      var page = this.page++
      setTimeout(() => {
        this.$axios
          .fetchPost('/portal', {
            interface: '4000',
            module: 'Investment',
            source: 'web',
            version: 'v1',
            data: {
              lastId: lastid,
              page: page
            }
          })
          .then(res => {
            console.log(res)
            this.lastId = res.data.lastId
            if (res.code == 0) {
              if (res.data.list.length == 0) {
                this.finished = true
              } else {
                var ret = res.data.list
                if (page == 1) {
                  this.list = ret
                } else {
                  for (var x in ret) {
                    this.list.push(ret[x])
                  }
                }
              }
            } else {
              this.finished = true
            }
            this.loading = false
          })
      }, 500)
    }
  },
  created () {

  }
}
</script>
<style scoped>
.mymill_bj {
  background: #0D0900;
  min-height: 100vh;
  height: auto;

}
.van-list{
  padding: 0 15px;
  box-sizing: border-box;
}
.mill_list {
  margin-top: 10px;
  padding: 5px 15px;
  background: #1D1C3B;
    /* padding: 0 10px; */
      border-radius: 8px;
      box-sizing: border-box;
}
.list {
  display: flex;
  align-items: center;

}
.list-img {
  width: 110px;
  height: 110px;
  display: flex;
  align-items: center;
  margin-right: 15px;
}
/* .list-img img{
  width: 80%;
  height: 80%;
} */
.intrudce {
  margin-left: 10px;
  display: flex;
  flex-direction: column;
  height: 110px;
  margin: 0;
}
.intrudce-p{
  align-self: flex-end;
  text-align: start;
}
.intrudce > p {
  margin: 5px 0 0 0;
}
.intrudce > p:nth-child(1) {
  color: #fff;
  font-size: 14px;
  font-weight: bold;
  
}
.intrudce-p > p:nth-child(1) {
  color: #FF1E1E;
  font-size: 14px;
  margin: 0;
}
.intrudce-p > p:nth-child(2) {
  color: #DEE7FF;
  font-size: 14px;
  text-indent: 2px;
  margin: 0;
}
.price {
  display: flex;
  align-items: center;
}
.price > p {
  margin: 5px 0 0 0;
  font-weight: 500;
  font-size: 13px;
}
.price > p:nth-child(1) {
  color: #333333;
}
.price > p:nth-child(2) {
  color: #f84d4d;
}


.van-hairline--top-bottom::after, .van-hairline-unset--top-bottom::after{
  border-width:0px 0px !important;
}
.van-cell__value{
            background: #0D0900;
            
        }
.van-field__control{
            color: #f8f8f8;
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
