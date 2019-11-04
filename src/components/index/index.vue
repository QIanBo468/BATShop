<template>
 <div id="index">
        <div class="shoptitle">
            <h2>{{title}}</h2>
        </div>
   <div class="zhanshitu">

          <van-swipe :autoplay="3000" style="width: 100%">
            <van-swipe-item v-for="(image, index) in images" :key="index">
              <img v-lazy="image.src" style="width: 100%;height: 100%" />
            </van-swipe-item>
          </van-swipe>
   </div>
   <div class="shopimg">
            <!--<img src="../../assets/shopimg_index/banner.png" alt="BAT商城">-->
        </div>
        <div class="shopmore">
            <div class="notice">
                <img style="float: left;" src="../../assets/shopimg_index/tz.png" alt="">
                <p>{{bannerAd}}</p>
            </div>
            <div class="more" @click="moreNotice">
                <img src="../../assets/shopimg_index/shopmore.png" alt="">
            </div>
        </div>
        <div class="shoplist">
            <div class="lsit" v-for="item of commodity" :key="item.id" @click="listShop(item)">
                <div class="listimg" style="height: 100%">
                    <img v-bind:src="item.thumb" alt="" style="width: 100%;height: 100%;">
                    <!-- <img src="../../assets/shopimg_index/kuangji.png" alt=""> -->
                </div>
                <div class="listcontent">
                    <h3 class="name">{{item.title}}</h3>
                    <p class="jianjie">{{item.description}}</p>
                    <div class="jifen">积分：
                        <span>{{item.price}}</span></div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
import Barrage from 'barrage-ui';
import example from 'barrage-ui/example.json';

import { Toast } from 'vant';
import text from '../text'
export default {
    data () {
        return {
          title: 'BAT商城',
          bannerAd: '',
          page:0,
          lastId:0,
          cpage:0,
          clastId:0,
          images:'',
          commodity: [
              {thumb:'../../assets/shopimg_index/kuangji.png',title:'123',description:'123123',price:'456465'},
              {thumb:'../../assets/shopimg_index/kuangji.png',title:'123',description:'123123',price:'456465'},
              {thumb:'../../assets/shopimg_index/kuangji.png',title:'123',description:'123123',price:'456465'},
            ]
        }
    },
    component: {
        text
    },
    mounted() {
    },
    created () {

    this.getGd();

      this.page = this.page +1;
      this.$axios.fetchPost('/portal/SimpleShop',
        {
          source: "web",
          version: "v1",
          module: "Goods",
          interface: "1000",
          data: {page:this.page,lastId:this.lastId}
        }).then(res => {
            console.log(res)
        if(res.success){
          this.page = res.data.currentPage;
          this.lastId = res.data.lastId;
          this.commodity = res.data.list;
        }
      })


      this.cpage = this.cpage +1;
        this.$axios.fetchPost('/portal',
        {
            source: "web",
            version: "v1",
            module: "Content",
            interface: "3000",
            data: {page:this.cpage,lastId:this.clastId}
        }).then(res => {
            if(res.success){
                this.bannerAd = res.data.list[0].title;

            }
        })

        
        /*this.$axios.fetchPost('/portal',
        {
            source: "web",
            version: "v1",
            module: "Attachment",
            interface: "2003",
        }).then(res => {
            // let text = ''
            // res.data.forEach((element, index) => {
            //     text += index+1 +'.' + element.title + '      '
            // });
            // console.log()
            if(res.success) this.rateObj = res.data
            
        })*/
    },
    methods : {

      moreNotice(){
        this.$router.push('./notice');
      },

      //获取轮播图

      getGd(){
        this.$axios.fetchPost('/portal',
          {
            source: "web",
            version: "v1",
            module: "Content",
            interface: "1000",
          }).then(res => {
          if(res.success) this.images = res.data.list

        })
      },


        listShop (item) {

            console.log(item)
            sessionStorage.setItem('shop' , item.id)
            this.$router.push({
                path: '/indexShop',
            })
        },
        message (obj) {
            if(obj.to){
                let  status = this.$cookies.get('status')
                if(status == -1){
                    this.$dialog.confirm({
                        title: '提示',
                        message: '未通过实名认证'
                    }).then(() => {
                        this.$router.push('/authentication')
                    }).catch(() => {

                    })
                    return
                }else if(status == -2){
                    this.$dialog.confirm({
                        title: '提示',
                        message: '未认证'
                    }).then(() => {
                        this.$router.push('/authentication')
                    }).catch(() => {
                        
                    })
                    return 
                }else if(status ==  0) {
                    Toast('认证已提交，后台审核中')
                    return
                }
                this.$router.push(obj.to)
            }else{
                Toast('正在开发中，敬请期待')
            }
        },
        deal(to) {
            let  status = this.$cookies.get('status')
            if(status == -1){
                this.$dialog.confirm({
                    title: '提示',
                    message: '未通过实名认证'
                }).then(() => {
                    this.$router.push('/authentication')
                }).catch(() => {

                })
                return
            }else if(status == -2){
                this.$dialog.confirm({
                    title: '提示',
                    message: '未认证'
                }).then(() => {
                    this.$router.push('/authentication')
                }).catch(() => {
                    
                })
                return 
            }else if(status ==  0) {
                Toast('认证已提交，后台审核中')
                return
            }
            this.$router.push(to)
        },
        createDM () {
            var that = this
            const barrage = new Barrage({
            container: document.getElementById('box'), // 父级容器
            data: that.classList, // 弹幕数据
            config: {
                // 全局配置项
                duration: 15000, // 弹幕循环周期(单位：毫秒)
                defaultColor: '#e6e6e6', // 弹幕默认颜色
                fontSize: 14,
            },
            avoidOverlap: false,
            });

            // 播放弹幕
            barrage.play();
        }
    }
}
</script>
<style lang="less">
    .tabBox{
        .van-grid-item__content{
            padding: 5px 6px;
        }
    }
    .connected .van-grid-item__content{
        padding: 0;
    }

</style>
<style lang="less" scoped>

  .zhanshitu{
    background: #1d1e3d;
    width: 100%;

    display: flex;
    justify-content: center;
    align-items: center;
    border-radius: 5px;
  img{
    width: 40%;
  }
  }

#index{
    width: 100%;
    height: 100%;
    overflow-y: auto;
    display: flex;
    flex-direction: column;
    // justify-content: center;
    align-items: center;
    background: rgb(11, 12, 33);
    .shoptitle{
        color: #ffffff
    }
    .shopmore{
        margin-top: 10px;
        margin-bottom: 10px;
        width: 100%;
        display: flex;
        justify-content: space-around;
        align-items: center;
        .notice{
            display: flex;
            height: 20px;
            /*justify-content: center;*/
            align-items: center;
            padding: 10px;
            color: #eee;
            width: 65%;
            background: #121e4d;
            border-radius: 30px;
            font-size: 14px;
            p{
             overflow: hidden;
            white-space: nowrap;
            text-overflow: ellipsis;               
            }
            overflow: hidden;
            white-space: nowrap;
            text-overflow: ellipsis;
        }
        .more{
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 20px;
            background: #a04ce2;
            width: 70px;
            height: 40px;;
        }
    }
    .shoplist{
        display: flex;
        justify-content: flex-start;
        flex-wrap: wrap;
        width: 100%;
        /*padding-left: 15px;
        padding-right: 15px;*/
        letter-spacing:1px;
        padding-left: 15px;
        .lsit{
            box-sizing: border-box;
            height: 200px;
            width: 45%;
            margin: 5px 5px;
            // padding-left: 10px;
            background: #212243;
            display: flex;
            flex-direction: column;
            justify-content: start;
            // align-items: center;
            .listimg{
                width: 100%;
                img{
                width: 100%;
                height: 50%;
                }
                
            }
            .name{
                color:#c4c5cd;
                font-size: 16px;
                margin: 5px;
            }
            .jianjie{
                color: #bec6e4;
                margin: 5px;
            }
            .jifen{
                margin: 5px;
                color: #FB4B48;
                span{
                    font-size: 16px;
                    font-weight: bold;
                }
            }
        }
    }
}
.hold{
    background: #f8f8f8;
    height: 10px;
}
.banner{
    width: 100%;
    height: 200px;
    background: url('../../../static/images/index/banner.png');
    background-size: 100% 100%;
    .bannerTitle{
        padding-top: 29px;
        text-align: center;
        height: 25px;
        line-height: 25px;
        color: #fff;
        font-size: 18px;
    }
}
.imgIcon{
    width:22px;
    height: 22px;
}
.tabBox{
    margin: 20px 0 29px;
}
.present{
    height: 154px;
    padding: 5px 16px;
    box-sizing: border-box;
    .title{
        height: 45px;
        line-height: 45px;
        border-bottom: 1px solid #eee;
        font-size: 18px;
    }
    .box_sm{
        .list{
            height: 22px;
            display: flex;
            align-items: center;
            font-size: 15px;
            padding: 15px 0;
            img{
                height: 22px;
                width: 22px;
                margin-right: 10px;
            }
            .red{
                color: #FB4B48;
                margin-left: 6px;
            }
        }
    }
}
.connected{
    // width: 343px;
    // padding: 18px 16px;
    margin: 10px 0;
}
</style>
