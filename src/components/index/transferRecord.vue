<template>
    <div id='transferRecord'>
         <van-nav-bar
            title="转账记录"
            left-arrow
            :border="false"
            @click-left='$router.go(-1)'
        />
        <div class='box'>
            <van-list
            v-model="loading"
            :finished="finished"
            finished-text="没有更多了"
            @load="onLoad"
            >
                <div class='record' v-for='(item, index) in list' :key='index'>
                    <div v-if='item.status == 0' class='status red'>转账中</div>
                    <div v-if='item.status == 1' class='status'>转账成功</div>
                    <div v-if='item.status == 2' class='status red'>转账失败</div>
                    <ul>
                        <!-- <li>
                            <div>会员ID</div>
                            <div>{{item.toUserId}}</div>
                        </li> -->
                        <li>
                            <div>提交时间</div>
                            <div>{{item.createdAt}}</div>
                        </li>
                        <!-- <li>
                            <div>钱包类型</div>
                            <div>{{item.creditName}}</div>
                        </li> -->
                        <!-- <li v-if='type == "ofc"'>
                            <div>手续费</div>
                            <div>{{item.fee}}</div>
                        </li>
                        <li v-if='type == "ofc"'>
                            <div>爱心基金</div>
                            <div>{{item.love}}</div>
                        </li> -->
                        <li>
                            <div>转账数量</div>
                            <div class='red'>{{item.money}}</div>
                        </li>
                    </ul>
                </div>
            </van-list>
        </div>
    </div>
</template>
<script>
export default {
    data () {
        return {
            list: [],
            finished: false,
            loading: false,
            page: 1,
            lastPage: null,
            lastId: 0,
            status: '',
            type: '',
        }
    },
    created() {
        var type = this.$route.query.type,
        status = '3102'
        if(type == 'BAT'){
            status = '2002'
        }else if(type == 'ofc'){
            status = '3100'
        }
        this.status = status
        this.type = this.$route.query.type
    },
    methods: {
        onLoad() {
            if (this.lastPage && this.lastPage < this.page) {
                this.finished = true
                this.loading = false;
            }else{
                
                this.$axios.fetchPost('/portal',
                {
                    source: "web",
                    version: "v1",
                    module: "Finance",
                    interface:  2002,
                    data: {lastId: this.lastId,page: this.page ++,isOut: true}
                }).then(res => {
                    console.log(res);
                    
                    this.list = this.list.concat(res.data.list)
                    this.lastPage = res.data.lastPage
                    this.loading = false;
                    this.lastId = res.data.lastId
                })
            }
        },
    }
}
</script>
<style lang="less">
    #transferRecord{
        display: flex;
        flex-direction: column;
        width: 100%;
        height: 100%;
        overflow: hidden;
        background: #000;
        .box{
            padding: 16px;
            overflow: scroll;
            .record{
                padding: 20px;
                border-radius: 6px;
                background: #fff;
                margin-bottom: 10px;
                .status{
                    font-size: 12px;
                    text-align: right;
                    color: #1890FF;
                    margin-bottom: 5px;
                }
                .red{
                    color: #F84D4D;
                }
                ul li{
                    height: 17px;
                    line-height: 17px;
                    font-size: 12px;
                    display: flex;
                    margin-bottom: 5px;
                    color: #fff;
                    div:first-child{
                        width: 60px;
                        color: #666666;
                        margin-right: 35px;
                    }
                    div:last-child{
                        width: 70%;
                    }
                }
                li:last-child{
                    margin-bottom: 0px;
                }
            }
        }
                .van-nav-bar{
            background: #000;
            // background:linear-gradient(180deg,#3FCFFE 0%,#39B2F8 100%);
            // background: #fff;
            .van-icon {
                color: #fff;
            }
            .van-nav-bar__title{
                color: #fff;
            }
            .van-nav-bar__text{
                color: #fff;
            }
        }
        .van-hairline--bottom::after{
            border: none;
        }
    }
</style>