<template>
    <!-- <div id='index'>
        <p>asdasdasd</p>
        <div id='box' class='banner'>
            <div class='bannerTitle'>首页</div>
        </div>
        <text ></text>
        <van-grid :column-num="4" class='tabBox' :border='false' >
            <van-grid-item
                v-for="(value, index) in List"
                :key="index"
                @click='message(value)'
            >
            <van-image :src="value.icon" width="47px"/>
                <div style="font-size: 12px;margin-top: 8px">{{value.text}}</div>
            </van-grid-item>
        </van-grid>
        <van-notice-bar
        :text="text"
        left-icon="volume-o"
        color="#111"
        background="#fff">
        <template slot='left-icon'>
            <img class='imgIcon' src="../../../static/images/index/gonggao.png" alt="">
        </template>
        <template slot='right-icon'>
            <img class='imgIcon' src="../../../static/images/index/more.png" alt="" @click='$router.push("/notice")'>
        </template>
        </van-notice-bar>
        <div class="hold"></div>
        <van-grid :column-num="2" class='connected' :border='false'>
            <van-grid-item
                v-for="(value, index) in userList"
                :key="index"
            >
                <a @click="deal(value.to)" >
            <div style="height:62px;display:flex;width: 112px;align-items: center;color: #111">
                    <van-image :src="value.icon" width="50px" height="50px"/>
                    <div style="margin-left: 10px;font-size: 13px">{{value.title}}</div>
            </div>
                </a>
            </van-grid-item>
        </van-grid>
        <div class="hold"></div>
        <div class="present">
            <div class="title">ofc当前价格</div>
            <div class='box_sm'>
                <div class="list">
                    <img src="../../../static/images/index/ofc.png" alt="">
                    <span>1.00000000 ofc ≈ </span>
                    <span class='red'> {{rateObj.CNY}} CNY</span>
                </div>
                <div class="list">
                    <img src="../../../static/images/index/ofc.png" alt="">
                    <span>1.00000000 ofc ≈ </span>
                    <span class='red'>{{rateObj.USDT}} usdt</span>
                </div>
                
            </div>
        </div>
        <div class="hold"></div>
    </div> -->
    <div id="index">
        <div class="shoptitle">
            <h2>{{title}}</h2>
        </div>
        <div class="shopimg">
            <img src="../../assets/shopimg_index/banner.png" alt="BAT商城">
        </div>
        <div class="shopmore">
            <div class="notice">
                <img src="../../assets/shopimg_index/tz.png" alt="">
                <p>{{bannerAd}}</p>
            </div>
            <div class="more">
                <img src="../../assets/shopimg_index/shopmore.png" alt="">
            </div>
        </div>
        <div class="shoplist">
            <div class="lsit" v-for="item of commodity" :key="item.id" @click="listShop(item)">
                <div class="listimg">
                    <img v-bind:src="item.imgsrc" alt="">
                    <!-- <img src="../../assets/shopimg_index/kuangji.png" alt=""> -->
                </div>
                <div class="listcontent">
                    <h3 class="name">{{item.name}}</h3>
                    <p class="jianjie">{{item.jianjie}}</p>
                    <div class="jifen">积分：
                        <span>{{item.jifen}}</span></div>
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
            bannerAd: '我是公告我是公告我是公告我是公告',
            commodity: [
                {id:0, name: '我是商品名称', jianjie: '我是商品简介', jifen: 123, imgsrc:require('../../assets/shopimg_index/kuangji.png')},
                {id:1, name: '我是商品名称', jianjie: '我是商品简介', jifen: 123, imgsrc:require('../../assets/shopimg_index/kuangji.png')},
                {id:2, name: '我是商品名称', jianjie: '我是商品简介', jifen: 123, imgsrc:require('../../assets/shopimg_index/kuangji.png')},
                {id:3, name: '我是商品名称', jianjie: '我是商品简介', jifen: 123, imgsrc:require('../../assets/shopimg_index/kuangji.png')}
            ]
        }
    },
    component: {
        text
    },
    mounted() {
    },
    created () {
        // let  status = this.$cookies.get('status')
        // if(status == -1){
        //     this.$dialog.confirm({
        //         title: '提示',
        //         message: 未通过实名认证
        //     }).then(() => {
        //         this.$router.push('/authentication')
        //     })
        //     return
        // }else if(status == -2){
        //     this.$dialog.confirm({
        //         title: '提示',
        //         message: '未认证'
        //     }).then(() => {
        //         this.$router.push('/authentication')
        //     })
        //     return 
        // }else if(status ==  0) {
        //     Toast('申请中')
        //     return
        // }
        this.$axios.fetchPost('/portal',
        {
            source: "web",
            version: "v1",
            module: "Content",
            interface: "4005",
            data: {}
        }).then(res => {
            if(res.success){
                let text = []
                res.data.forEach((element, index) => {
                    text.push({
                        text: element,
                        key: index,
                        time: 1000*Math.floor(Math.random()*10)
                    })
                });
                this.classList = text
                this.createDM ()
            }
        })
        this.$axios.fetchPost('/portal',
        {
            source: "web",
            version: "v1",
            module: "Content",
            interface: "4004",
            data: {}
        }).then(res => {
            // let text = ''
            // res.data.forEach((element, index) => {
            //     text += index+1 +'.' + element.title + '      '
            // });
            // console.log()
            if(res.success) this.text = res.data[0].title
            
        })
        
        this.$axios.fetchPost('/portal',
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
            
        })
    },
    methods : {
        listShop (item) {
            this.$router.push({
                path: '/indexShop',
                query:{
                    item: item
                }
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
            justify-content: center;
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
        justify-content: space-around;
        flex-wrap: wrap;
        width: 100%;
        padding-left: 15px;
        padding-right: 15px;
        letter-spacing:1px;
        .lsit{
            box-sizing: border-box;
            height: 200px;
            width: 45%;
            margin-top: 10px;
            padding-left: 10px;
            background: #212243;
            .listimg{
                display: flex;
                justify-content: center;
                align-items: center;
                width: 100%;
                height: 50%;
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