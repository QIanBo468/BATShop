<template>
    <div id='conversion'>
        <van-nav-bar
            title="充币记录"
            left-arrow
            :border="false"
            @click-left="$router.go(-1)"
        />
        <div class='box'>
            <van-list
            v-model="loading"
            :finished="finished"
            finished-text="没有更多了"
            @load="onLoad"
            >
                <div class='record' v-for='(item, index) in list' :key='index'>
                <div class='status' :class='item.status==1 ? "active" : ""'>{{statusArr[item.status]}}</div>
                <ul>
                    <li>
                        <div>充币地址</div>
                        <div>{{item.address}}</div>
                    </li>
                    <li>
                        <div>提交时间</div>
                        <div>{{item.createdAt}}</div>
                    </li>
                    <li>
                        <div>充币数量</div>
                        <div class='red'>{{item.amount}}</div>
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
            statusArr:{
                '-1':'已驳回',
                '0': '充值中',
                '1': "已通过",
            }
        }
    },
    methods: {
        onLoad() {
            if (this.lastPage && this.lastPage < this.page) {
                this.finished = true
                this.loading = false;
            }else{
                
                this.$axios.fetchPost('/portal/digiccy',
                {
                    source: "web",
                    version: "v1",
                    module: "Wallet",
                    interface:  1002,
                    data: {lastId: this.lastId,page: this.page ++,isOut: true}
                }).then(res => {
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
    #conversion{
        display: flex;
        flex-direction: column;
        width: 100%;
        height: 100%;
        overflow: hidden;
        background: #f8f8f8;
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
                    color: #F84D4D;
                    margin-bottom: 10px;
                }
                .active{
                    color: #1890FF;
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
                    div:first-child{
                        width: 60px;
                        color: #666666;
                        margin-right: 35px;
                    }
                    div:last-child{
                        width: 70%;
                        overflow: hidden;
                        text-overflow:ellipsis;
                        white-space: nowrap;
                    }
                }
                li:last-child{
                    margin-bottom: 0px;
                }
            }
        }
    }
</style>