<template>
<div class="root">
  <van-nav-bar
    title="问题反馈"
    left-arrow
    class="nav-top"
    @click-left="back"
  >
    <template #right>
      <div class="nav-top-right" @click="goToFeedbackNew">我要反馈</div>
    </template>
  </van-nav-bar>

  <van-list
    v-model="loading"
    :finished="finished"
    finished-text="没有更多了"
    @load="load"
    class="list"
  >
    <div
      v-for="(item, i) in list"
      :key="i"
      class="item"
      @click="clickItem(item)"
    >
      <div class="item-top">
        <img class="item-top-img" src="../../assets/img/question.png" alt="ask" />
        <div class="item-top-feedback">
          <div>{{ item.content }}</div>
          
          <ul class="item-top-img-list">
            <li
              v-for="(img, i) in item.imageList"
              :key="i"
            >
              <img class="item-top-img-item" :src="img" alt="img" />
            </li>
          </ul>

          <div class="item-top-date">{{ item.createdAt }}</div>
        </div>
      </div>

      <div class="item-bottom" v-if="item.reply">
        <div class="item-bottom-reply">
          <div>{{ item.reply }}</div>
          <div class="item-top-date">{{ item.replyAt }}</div>
        </div>
        <img class="item-top-img" src="../../assets/img/answer.png" alt="reply" />
      </div>
    </div>
  </van-list>

</div>
</template>

<script>
export default {
  data() {
    return {
      total: 0,
      list: [],
      loading: false,
      finished: false,
      page: 1,

      lastId: 0,
    }
  },

  methods: {
    back() {
      this.$router.go(-1)
    },

    load() {
      this.loading = true;

      this.getData(this.page);

      this.loading = false;
      this.page++;
    },

    clickItem(item) {
    },

    goToFeedbackNew() {
      this.$router.push("/feedbackNew")
    },

    getData(page) {
      this.$axios.fetchPost('/portal', {
        source: "web",
        version: "v1",
        module: "Content",
        interface: "5000",
        data: {
          lastId: this.lastId,
          page,
        }
      }).then(res => {
        console.log(res)
        this.lastId = res.data.lastId
        let list = res.data.list
        this.total = res.data.total
        this.list.push(...list)

        if (this.list.length >= this.total) {
          this.finished = true
        }
      })

      /* this.list = [
        {
          content: "可不可以增加矿机的种类呢",
          createdAt: "2019.06.12 13:00:54",
          reply: "明天上新哦，敬请期待！",
          replyAt: "2019.06.12 13:00:54",
        },
        {
          content: "请问这个是什么意思？",
          createdAt: "2019.06.12 13:00:54",
          imageList: [
            "http://img2.mtime.com/up/454/1152454/1382C875-EF8E-4B41-A5AF-CF920AABC062_500.jpg",
            "http://img2.mtime.com/up/454/1152454/1382C875-EF8E-4B41-A5AF-CF920AABC062_500.jpg",
            "http://img2.mtime.com/up/454/1152454/1382C875-EF8E-4B41-A5AF-CF920AABC062_500.jpg",
          ],
          reply: "亲，这是正在处理中的意思，请耐心等待！",
          replyAt: "2019.06.12 13:00:54",
        },
      ] */
    },
  },
}
</script>

<style lang="less" scoped>
.root {
  background: #f8f8f8;
  height: 100vh;
}

.nav-top {
  margin-bottom: 10px;
  
  &-right {
    color: #666;
  }
}

.list {
  margin: 15px;
}

.item {
  padding: 14px 10px;
  border-bottom: 1px solid #eee;
  border-radius: 6px;
  font-size: 14px;
  color: #333;
  background: #fff;
  margin-bottom: 10px;

  &:active {
    background: #f2f3f5;
  }

  &-top {
    display: flex;
    padding-bottom: 8px;

    &-img-list {
      display: flex;
    }
    &-img-item {
      width: 70px;
      height: 70px;
      margin: 6px 6px 0 6px;
    }

    &-feedback {
      display: flex;
      flex-direction: column;
      color: #666;
    }

    &-date {
      margin-top: 5px;
      color: #999;
      font-size: 12px;
    }

    &-img {
      width: 20px;
      height: 20px;
      margin: 0 5px;
    }
  }

  &-bottom {
    display: flex;
    justify-content: flex-end;
    padding-top: 8px;
    border-top: 1px solid #f8f8f8;

    &-reply {
      display: flex;
      flex-direction: column;
      align-items: flex-end;
      color: #666;
    }
  }

}

</style>
