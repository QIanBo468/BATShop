<template>
  <div class="newaddress">
    <van-nav-bar title="添加地址" left-arrow @click-left="onClickLeft" />
    <!-- <div class="new_add">
      <div class="add">
        <div class="adds">
          <p>收货人</p>
          <input type="text" placeholder="请输入姓名" v-model="name" />
        </div>
        <div class="adds">
          <p>手机号</p>
          <input type="text" placeholder="请输入有效地址" v-model="address" />
        </div>
        <div class="adds">
          <p>所在地区</p> -->
         <van-address-edit
            :area-list="areaList"
           
            :search-result="searchResult"
            area-columns-placeholder="['请选择', '请选择', '请选择']"
            @save="onSave"
            @delete="onDelete"
            
          />

          <!-- <input type="text" placeholder="请输入有效地址" v-model="address" /> -->
        </div>
      <!-- </div>
      <div class="sure">
        <button @click="submit">保存</button>
      </div>
    </div> -->
  <!-- </div> -->
</template>

<script>

import { Area } from 'vant'
import areaList from './area.js'
import { Toast } from "vant"

export default {
  data() {
    return {
      show: false,
      areaList,
      searchResult: [],
      name: "",
      address: ""
    };
  },
  computed: {},
  methods: {
    onClickLeft() {
      this.$router.go(-1);
    },
    onSave(content) {
      // Toast('save');
      console.log(content)
    },
    onDelete() {
      Toast('delete');
    },
    // onChangeDetail(val) {
    //   if (val) {
    //     this.searchResult = [{
    //       name: '黄龙万科中心',
    //       address: '杭州市西湖区'
    //     }];
    //   } else {
    //     this.searchResult = [];
    //   }
    // },
    submit() {
      this.$axios
        .fetchPost("/portal", {
          interface: "8001",
          module: "User",
          source: "web",
          version: "v1",
          data: {
            name: this.name,
            address: this.address
          }
        })
        .then(res => {
          console.log(res);
          if (res.code == 0) {
            this.$toast({
              message: res.message,
              duration: 1000
            });
            setTimeout(() => {
              this.$router.go(-1);
            }, 1000);
          } else {
            this.$toast(res.message);
          }
        });
    }
  },
  created() {}
};
</script>
<style lang='less' scoped>

.newaddress {
  width: 100%;
  height: 100%;
  background: #0B0C21;
}
.new_add {
  background: #fff;
  margin-top: 10px;
  height: 100%;
  position: relative;
}
.add {
  padding: 20px;
}
.adds > p {
  color: #666666;
  font-size: 14px;
}
.adds > input {
  width: 100%;
  height: 40px;
  border: none;
  border: 1px solid #d8d8d8;
  border-radius: 4px;
  padding: 0 0 0 7px;
}
// .adds >>> input::placeholder {
//   color: #999999;
//   font-size: 15px;
// }
.sure {
  padding: 15px;
  margin-top: 55px;
}
button {
  background: #f45c68;
  width: 100%;
  border: none;
  padding: 15px;
  color: #fff;
  font-size: 16px;
  border-radius: 23px;
}
/deep/.van-address-edit__buttons{
  width: 100%;
  position: fixed;
  bottom: 0;
  display: flex;
  justify-content: center;
  padding: 0;
}
/deep/.van-field__control{
  color: #f8f8f8;
}
/deep/.van-button--danger{
  width: 90%;
  background-image: linear-gradient(90deg,#494efe, #0b02f8);
  border: none;
  border-radius: 8px;
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
/deep/.van-cell{
  background: #1D1E3D;
  border: none !important;
  color: #f8f8f8;
}
/deep/.van-cell:not(:last-child)::after{
 border-bottom: none;
}
</style>
