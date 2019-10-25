<template>
<div class="root">
  <van-nav-bar title="设置" left-arrow class="nav-top" @click-left="back" />

  <van-cell-group class="group">
    <div class="cell select-image-parent">
      <span>头像</span>
      <div class="cell-right">
        <input
          class="select-image"
          type="file"
          accept="image/*"
          capture="camera"
          @change="uploadImage"
        />

        <img v-if="headimg !== ''" class="cell-right-img" :src="headimg" alt="头像"/>
        <img v-else class="cell-right-img" src="../../assets/img/morentouxiang.png" alt="头像"/>
        <van-icon class="cell-right-icon" name="arrow"></van-icon>
      </div>
    </div>
    <div class="cell" @click="clickNickName">
      <span>昵称</span>
      <div class="cell-right">
        <span class="cell-right-text">{{ nickname }}</span>
        <van-icon class="cell-right-icon" name="arrow"></van-icon>
      </div>
    </div>
  </van-cell-group>

  <van-cell-group class="group">
    <div class="cell" @click="clickAbout">
      <span>关于我们</span>
      <div class="cell-right">
        <van-icon class="cell-right-icon" name="arrow"></van-icon>
      </div>
    </div>
  </van-cell-group>
  <!-- <div class='btn' @click='submit'>退出登录</div> -->
</div>
</template>

<script>

import { Exif } from './exif.js'

export default {
  data() {
    return {
      headimg: "",
      nickname: "",
    }
  },

  methods: {
    submit () {
      this.$axios.fetchPost('/portal', {
        source: "web",
        version: "v1",
        module: "Account",
        interface: "2000",
        data: {}
      }).then(res => {
        if(res.success) {
          this.$router.push('/login')
        }
      })
    },
    getData() {
      this.$axios.fetchPost('/portal', {
        source: "web",
        version: "v1",
        module: "User",
        interface: "1000",
        data: {}
      }).then(res => {
        // console.log(res)
        this.nickname = res.data.nickname
        this.headimg = res.data.avatar
      })
    },

    back() {
      this.$router.go(-1)
    },

    uploadImage(e) {


// connectCamera(){
//   navigator.getMedia = navigator.getUserMedia || navigator.webkitGetUserMedia || navigator.mozGetUserMeddia || navigator.msGetUserMedia;
//   let that=this;
//   if (navigator.mediaDevices && navigator.mediaDevices.getUserMedia) {
//     navigator.mediaDevices.getUserMedia({
//       video: true,
//       audio: true
//     }).then(function(stream) {
//       console.log(stream);
//       that.stream=stream;
//       that.cameraState=true;
//       that.stream = typeof stream.stop === 'function' ? stream : stream.getTracks()[1];
//       console.log(that.stream);
//       var smallVideo=$(".smallVideo")[0];
//       var bigVideo=$(".bigVideo")[0];
//       smallVideo.src = (window.URL || window.webkitURL).createObjectURL(stream);
//       bigVideo.src = (window.URL || window.webkitURL).createObjectURL(stream);
//       smallVideo.play();
//       bigVideo.play();
 
//     }).catch(function(err) {
//       console.log(err);
//     })
//   }
//   // 使用旧方法打开摄像头
//   else if (navigator.getMedia) {
//     navigator.getMedia({
//       video: true
//     }, function(stream) {
 
//       console.log(stream);
//       that.stream=stream;
//       that.cameraState=true;
//       that.stream = stream.getTracks()[0];;
//       console.log(that.stream);
//       var smallVideo=$(".smallVideo")[0];
//       var bigVideo=$(".bigVideo")[0];
//       smallVideo.src = (window.URL || window.webkitURL).createObjectURL(stream);
//       bigVideo.src = (window.URL || window.webkitURL).createObjectURL(stream);
//       smallVideo.play();
//       bigVideo.play();
//     }, function(err) {
//       console.log(err);
//     });
//   }
// },
 
// //关闭摄像头
// closeCamera(){
//   this.cameraState=false;
//   this.stream && this.stream.stop();
// },
 
// //点击拍摄或集鸽拍摄
// reCamera(foot_ring_sn){
//   console.log("拍照调用");
//   var smallCanvas=$(".smallCanvas")[0];
//   var smallContext=smallCanvas.getContext("2d");
//   let smallVideo=$(".smallVideo")[0];
//   smallContext.drawImage(smallVideo, 0, 0, 270, 200);
//   //拍好的图片显示
//   //smallCanvas.toDataURL('image/png').split(",")[1]
// },

      let file = e.target.files[0]
      if (!file) {
        return
      }

      let reader = new FileReader()
      reader.onload = (e) => {
        this.headimg = e.target.result
      }
      reader.readAsDataURL(file)

      let form = new FormData()
      form.append("file", file)
      this.$axios.fetchPost("http://bat.qdunzi.com/upload", form).then(res => {
        // console.log(res)
        this.$axios.fetchPost('/portal', {
          source: "web",
          version: "v1",
          module: "User",
          interface: "1001",
          data: {
            avatar: res.data.file,
          }
        }).then(res => {
          // console.log(res)
          this.$toast("更换头像成功")
        })
      }).catch(err => {
        console.log(err)
      })
    },
// upload (e) {




//    let files = e.target.files || e.dataTransfer.files;
//    if (!files.length) return;
//    this.picValue = files[0];
//    this.imgPreview(this.picValue);
//    console.log(this.picValue)
//   },
//   imgPreview (file) {
//    let self = this;
//    let Orientation;
//    //去获取拍照时的信息，解决拍出来的照片旋转问题
//     Exif.getData(file, function(){
//       Orientation = Exif.getTag(this, 'Orientation');
//     });
//    // 看支持不支持FileReader 
//    if (!file || !window.FileReader) return;
 
//    if (/^image/.test(file.type)) {
//      // 创建一个reader
//      let reader = new FileReader();
//      // 将图片2将转成 base64 格式
//      reader.readAsDataURL(file);
//      // 读取成功后的回调
//      reader.onloadend = function () {
//       let result = this.result;
//       let img = new Image();
//       img.src = result;
//       //判断图片是否大于100K,是就直接上传，反之压缩图片
//       if (this.result.length <= (100 * 1024)) {
//        self.headerImage = this.result;
//        self.postImg();
//       }else {
//        img.onload = function () {
//         let data = self.compress(img,Orientation);
//         self.headerImage = data;
//         self.postImg();
//        }
//       }
//      }
//     }
//    },
//    postImg () {
//     //这里写接口
//     this.$axios.fetchPost("http://ofc.qdunzi.com/upload", form).then(res => {
//         // console.log(res)
//         this.$axios.fetchPost('/portal', {
//           source: "web",
//           version: "v1",
//           module: "User",
//           interface: "1001",
//           data: {
//             avatar: res.data.file,
//           }
//         }).then(res => {
//           // console.log(res)
//           this.$toast("更换头像成功")
//         })
//       }).catch(err => {
//         console.log(err)
//       })
//    },
//    rotateImg (img, direction,canvas) {
//     //最小与最大旋转方向，图片旋转4次后回到原方向
//     const min_step = 0;
//     const max_step = 3;
//     if (img == null)return;
//     //img的高度和宽度不能在img元素隐藏后获取，否则会出错
//     let height = img.height;
//     let width = img.width;
//     let step = 2;
//     if (step == null) {
//       step = min_step;
//     }
//     if (direction == 'right') {
//       step++;
//       //旋转到原位置，即超过最大值
//       step > max_step && (step = min_step);
//     } else {
//       step--;
//       step < min_step && (step = max_step);
//     }
//     //旋转角度以弧度值为参数
//     let degree = step * 90 * Math.PI / 180;
//     let ctx = canvas.getContext('2d');
//     switch (step) {
//      case 0:
//        canvas.width = width;
//        canvas.height = height;
//        ctx.drawImage(img, 0, 0);
//        break;
//      case 1:
//        canvas.width = height;
//        canvas.height = width;
//        ctx.rotate(degree);
//        ctx.drawImage(img, 0, -height);
//        break;
//      case 2:
//        canvas.width = width;
//        canvas.height = height;
//        ctx.rotate(degree);
//        ctx.drawImage(img, -width, -height);
//        break;
//      case 3:
//        canvas.width = height;
//        canvas.height = width;
//        ctx.rotate(degree);
//        ctx.drawImage(img, -width, 0);
//        break;
//     }
//   },
//   compress(img,Orientation) {
//    let canvas = document.createElement("canvas");
//    let ctx = canvas.getContext('2d');
//     //瓦片canvas
//    let tCanvas = document.createElement("canvas");
//    let tctx = tCanvas.getContext("2d");
//    let initSize = img.src.length;
//    let width = img.width;
//    let height = img.height;
//    //如果图片大于四百万像素，计算压缩比并将大小压至400万以下
//    let ratio;
//    if ((ratio = width * height / 4000000) > 1) {
//     console.log("大于400万像素")
//     ratio = Math.sqrt(ratio);
//     width /= ratio;
//     height /= ratio;
//    } else {
//     ratio = 1;
//    }
//    canvas.width = width;
//    canvas.height = height;
//  //    铺底色
//    ctx.fillStyle = "#fff";
//    ctx.fillRect(0, 0, canvas.width, canvas.height);
//    //如果图片像素大于100万则使用瓦片绘制
//    let count;
//    if ((count = width * height / 1000000) > 1) {
//     console.log("超过100W像素");
//     count = ~~(Math.sqrt(count) + 1); //计算要分成多少块瓦片
//  //      计算每块瓦片的宽和高
//     let nw = ~~(width / count);
//     let nh = ~~(height / count);
//     tCanvas.width = nw;
//     tCanvas.height = nh;
//     for (let i = 0; i < count; i++) {
//      for (let j = 0; j < count; j++) {
//       tctx.drawImage(img, i * nw * ratio, j * nh * ratio, nw * ratio, nh * ratio, 0, 0, nw, nh);
//       ctx.drawImage(tCanvas, i * nw, j * nh, nw, nh);
//      }
//     }
//    } else {
//     ctx.drawImage(img, 0, 0, width, height);
//    }
//    //修复ios上传图片的时候 被旋转的问题
//    if(Orientation != "" && Orientation != 1){
//     switch(Orientation){
//      case 6://需要顺时针（向左）90度旋转
//        this.rotateImg(img,'left',canvas);
//        break;
//      case 8://需要逆时针（向右）90度旋转
//        this.rotateImg(img,'right',canvas);
//        break;
//      case 3://需要180度旋转
//        this.rotateImg(img,'right',canvas);//转两次
//        this.rotateImg(img,'right',canvas);
//        break;
//     }
//    }
//    //进行最小压缩
//    let ndata = canvas.toDataURL('image/jpeg', 0.1);
//    console.log('压缩前：' + initSize);
//    console.log('压缩后：' + ndata.length);
//    console.log('压缩率：' + ~~(100 * (initSize - ndata.length) / initSize) + "%");
//    tCanvas.width = tCanvas.height = canvas.width = canvas.height = 0;
//    return ndata;
//  },

    clickNickName() {
      this.$router.push({
        path: "/settingChangeNickName",
        query: {
          nickname: this.nickname,
        },
      })
    },

    clickAbout() {
      this.$router.push("/settingAboutUs")
    },
  },

  created() {
    this.getData()
  },
}
</script>

<style lang="less" scoped>
.van-cell-group{
  background: #1D1C3B;
}
/deep/.van-hairline--top-bottom::after, .van-hairline-unset--top-bottom::after{
  border-width:0px 0px !important;
}
 /deep/.van-cell__value{
            background: #1D1C3B;
            
        }
        /deep/.van-field__control{
            color: #f8f8f8;
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
.root {
  background: #000;
  height: 100vh;
}

.nav-top {
  margin-bottom: 10px;
}

.group {
  margin-bottom: 10px;
}

.cell {
  height: 60px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  // border-bottom: 1px solid #f9f9f9;
  font-size: 14px;
  padding: 0 16px;
  // color: rgba(102, 102, 102, 1);
  color: #f8f8f8;

  &:active {
    background: #f2f3f5;
  }

  &-right {
    display: flex;
    align-items: center;
    justify-content: flex-end;
    max-width: 80%;

    &-img {
      width: 44px;
      height: 44px;
      border-radius: 22px;
    }

    &-text {
      color: #fff;
      width: 80%;
      line-height: 14px;
      flex-grow: 1;
    }

    &-icon {
      margin-left: 6px;
      color: #bbb;
      font-size: 16px;
    }
  }
}

.select-image-parent {
  position: relative;
}
.select-image {
  position: absolute;
  top: 0;
  bottom: 0;
  width: 100%;
  height: 100%;
  opacity: 0;
}
.btn{
   width: 343px;
    height: 44px;
    background: red;
    margin: 0 auto;
    background:linear-gradient(180deg,rgba(253,89,102,1) 0%,rgba(231,17,34,1) 100%);
    border-radius: 22px;
    color: #fff;
    text-align: center;
    line-height: 44px;
    font-size: 16px;
    margin-top: 300px;
}
</style>
