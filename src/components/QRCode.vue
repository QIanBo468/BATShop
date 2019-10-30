<template>
  <div id="qrCode">
      <div id="code"></div>
      <canvas id="canvas"></canvas>
  </div>
</template>

<script>

import QRCode from 'qrcode'

export default {
  props:{
    qrcode:'',
  },
data(){
    return {
        msg: 'hello vue',
        codes:''
            }
        },
      components: {
        QRCode: QRCode
      },

      methods: {
        useqrcode(){
          var canvas = document.getElementById('canvas')
          QRCode.toCanvas(canvas, this.qrcode, function (error) {
            if (error) console.error(error)
            console.log('QRCode success!');
          })
        }
      },

      mounted() {
        this.useqrcode();
      },
  watch:{
    qrcode(){
      this.useqrcode();
    }
  }
}
</script>

<style>

</style>
