<template>
  <div class="my_team">

    <van-nav-bar title="我的团队" left-arrow @click-left="onClickLeft" />
      <div class="header">
      <div class="tjnum">
        <span>推荐人数</span>
        <p>{{tjnum}}</p>
      </div>
      <div class="yeji">
        <span>团队业绩</span>
        <p>{{yeji}}</p>
      </div>
    </div>
    <van-list v-model="loading" :finished="finished" finished-text="没有更多了" @load="get_team">
      <div class="team_list" v-for="(item,index) in list" :key="index">
        <div class="team_person">
          <div class="personal">
            <img :src="item.avatar" alt class="picture" />
            <span>{{item.nickname}}</span>
          </div>
          <div class="team_id">
            <span>ID：{{item.id}}</span>
            <span>{{item.created_at}}</span>
          </div>
        </div>
      </div>
    </van-list>
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

        ],
      tjnum: 0,
      yeji: 0
    }
  },
  computed: {},
  methods: {
    onClickLeft () {
      this.$router.go(-1)
    },

    //查询团队业绩
    get_teamAward(){
      this.$axios
        .fetchPost('/portal', {
          interface: '4100',
          module: 'User',
          source: 'web',
          version: 'v1',
          data: {
          }
        })
        .then(res => {
          console.log(res)
          if (res.code == 0) {
            this.yeji = res.data.userInfo.performance
          }
        })
    },
    get_team () {
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
            interface: '4101',
            module: 'User',
            source: 'web',
            version: 'v1',
            data: {
              lastId: lastid,
              page: page,
              floor: 0
            }
          })
          .then(res => {
            console.log(res)
            this.lastId = res.data.lastId
            // this.list = res.data.list
            if (res.code == 0) {
              if (res.data.list.length == 0) {
                this.finished = true
              } else {
                var ret = res.data.list
                this.tjnum = res.data.total;
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
    this.get_teamAward();
  }
}
</script>
<style scoped>
.my_team {
  background: #0D0900;
  min-height: 100vh;
  height: auto;
}
.header{
  width: 90%;
  height: 100px;
  border-radius: 8px;
  margin: 0 auto;
  background-image: linear-gradient(90deg,#6868FF, #2329D5);
  display: flex;
  align-items: center;
}
.header .tjnum{
    width: 50%;
    height: 80%;
    border-right: .5px solid #6B6EFF;
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-around;
    color: #f8f8f8;
  }
  .header .tjnum span{
    font-size: 13px;
    color: #E9E9FF;
  }
  .header .tjnum p{
    font-size: 26px;
    margin: 0;
  }

  .header .yeji{
    width: 50%;
    height: 80%;
    /* border-right: .5px solid #6B6EFF; */
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: space-around;
    color: #f8f8f8;
  }
  .header .yeji span{
    font-size: 13px;
    color: #E9E9FF;
  }
  .header .yeji p{
    font-size: 26px;
    margin: 0;
  }
.team_list {
  padding: 15px;
  background: #1D1C3B;
  margin-top: 10px;
}

.team_person {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.team_id {
  display: flex;
  flex-direction: column;
}
.team_id > span:nth-child(1) {
  color: #FFFFFF;
  font-size: 14px;
}
.team_id > span:nth-child(2) {
  margin-top: 5px;
  color: #9AB5FF;
  font-size: 12px;
}
.personal {
  display: flex;
  align-items: center;
  
}

.picture {
  width: 44px;
  height: 44px;
  margin-right: 10px;
  border-radius: 50%;
}
.personal > span {
  color: #fff;
  font-size: 16px;
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
