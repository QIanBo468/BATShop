<template>
    <div id="mell">
         <van-nav-bar
            title="我的钱包"
            
        />
        <div class='list'>
            <router-link to='/usdt'>
                <img src="../../../static/images/index/yitai@3x.png" width="40px" height="40px" alt="">
                <div class='num'>
                    <div>{{usdt.label}}</div>
                    <div>{{usdt.value}}</div>
                </div>
                <img src="../../../static/images/index/in@3x(2).png" width="16px" height="16px" alt="">
            </router-link>
        </div>
        <div class='list ' >
            <router-link to='/ofc' class='bgyellow'>
                <img src="../../../static/images/index/usdt@3x.png" width="40px" height="40px" alt="">
                <div class='num'>
                    <div>{{ofc.label}}</div>
                    <div>{{ofc.value}}</div>
                </div>
                <img src="../../../static/images/index/in@3x(2).png" width="16px" height="16px" alt="">
            </router-link>
        </div>
        <div class='list '>
            <router-link to='/fund' class='bgred'>
            <img src="../../../static/images/index/laiwen@3x.png" width="40px" height="40px" alt="">
            <div class='num'>
                <div>{{LoveFund.label}}</div>
                <div>{{LoveFund.value}}</div>
            </div>
            <img src="../../../static/images/index/in@3x(2).png" width="16px" height="16px" alt="">
            </router-link>
        </div>
        
    </div>
</template>
<script>
import { Dialog, Toast } from 'vant'
export default {
     data () {
        return {
            usdt: {
                label: 'BAT钱包',
               value: 0.00
            },
            ofc: {
                name: '可用积分',
                have: 751098.12
            },
            LoveFund: {
                name: '消费积分',
                have: 751098.12
            },

        }
    },
    created () {
        this.$axios.fetchPost('/portal',
        {
            source: "web",
            version: "v1",
            module: "Finance",
            interface: "1000",
            data: {}
        }).then(res => {
            console.log(res)
            if (res.success) {
            this.usdt = res.data.credit_1
            this.ofc = res.data.credit_5
            this.fu = res.data.fu
            this.LoveFund = res.data.credit_3
            }

        })
    }
}
</script>
<style lang="less" scoped>
#mell{
    .van-nav-bar{
     background: #0D0900;
     .van-nav-bar__title{
     color: #fff; 
     }

    }
        background: #0D0900;
        .list{
            a{
                background:linear-gradient(180deg,#4C67FA 0%,#7382FC 100%);
                width: 343px;
                margin: 15px auto 0;
                height: 100px;
                border-radius: 6px;
                padding: 22px  20px 22px 15px;
                box-sizing: border-box;
                display: flex;
                align-items: center;
            // background:linear-gradient(180deg,rgba(255,202,49,1) 0%,rgba(247,107,28,1) 100%);
            
            .num{
                flex: 1;
                overflow: hidden;
                color: #fff;
                margin-left: 10px;
                div{
                    height: 22px;
                    line-height: 22px;
                    font-size: 16px;
                }
                div:last-child{
                    height: 32px;
                    line-height: 32px;
                    font-size: 23px;
                }
            }
            // img{
            //     width: 20px;
            // }
            }
        }
        .bgyellow{
            background:linear-gradient(180deg,#815EFD 0%,#B775FF 100%)!important
        }
        .bgred{
            background:linear-gradient(180deg,#FE7474 0%,#FDAA73 100%)!important;
        }
        .bggray{
            background:linear-gradient(180deg,#FE7474 0%,#FDAA73 100%)!important;
        }
    }
    [class*=van-hairline]::after{
        border: 0px solid #0D0900;
     }
    // #mall{
    //     width: 100%;
    //     height: 100%;
    //     overflow: hidden;
    //     background: #001672;
    //     display: flex;
    //     flex-direction: column;
    //     .title{
    //         height: 40px;
    //         text-align: center;
    //         width: 100%;
    //         line-height: 40px;
    //         font-size: 18px;
    //         background: rgba( 255, 255, 255, .05);
    //         // opacity: 0.5
    //         color: #fff;
    //     }
    //     .box{
    //         flex: 1;
    //         overflow-y: auto;
    //         margin-top: 10px;
    //         .goodList{
    //             margin-bottom: 10px;
    //             background:rgba(255,255,255, .05);
    //             height: 115px;
    //             box-sizing: border-box;
    //             padding: 12px 16px;
    //             display: flex;
    //             img{
    //                 width: 90px;
    //                 height: 90px;
    //                 margin-right: 10px;
    //             }
    //             ul{
    //                 flex: 1;
    //                 overflow: hidden;
    //                 padding-top: 10px;
    //                 box-sizing: border-box;
    //                 li{
    //                     height: 18px;
    //                     line-height: 18px;
    //                     color: #D8D8D8;
    //                     font-size: 13px;
    //                 }
    //                 li:first-child{
    //                     height: 24px;
    //                     margin-right: 5px;
    //                     line-height: 24px;
    //                     color: #fff;
    //                     font-size: 17px;
    //                 }
    //                 .red{
    //                     color: #F84D4D;
    //                 }
    //             }
    //             .btn_{
    //                 width: 76px;
    //                 margin-top: 44px;
    //                 height: 28px;
    //                 text-align: center;
    //                 line-height: 28px;
    //                 border-radius: 20px;
    //                 color:#fff;
    //                 background:linear-gradient(180deg,rgba(253,89,102,1) 0%,rgba(255,73,65,1) 52%,rgba(231,17,34,1) 100%);
    //             }
    //         }
    //     }
    // }
</style>
