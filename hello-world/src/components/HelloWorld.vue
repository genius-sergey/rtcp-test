<template>
  <div id="container" class="player">
    <div id="video-container" class="video"></div>
  </div>
</template>

<script>
const JSMpeg = require('jsmpeg')
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data(){
    return{
    }
  },
  async mounted(){
    let jsmpegPlayer
    const videoContainer = document.getElementById('video-container')

    const streamUrl = "rtsp://wowzaec2demo.streamlock.net/vod/mp4:BigBuckBunny_115k.mov"
    fetch('http://127.0.0.1:3009/start', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json;charset=utf-8',
      },
      body: JSON.stringify({ streamUrl }),
    })
    .then((response) => response.json())
    .then((data) => {
      if (jsmpegPlayer) {
        jsmpegPlayer.stop()
        jsmpegPlayer.destroy()
      }

      const canvas = document.createElement('canvas')
      canvas.id = 'jsmpeg-player'
      videoContainer.appendChild(canvas)

      jsmpegPlayer = new JSMpeg(data.url, {
        canvas,
        autoplay: true,
      })
    })
  }

}
</script>

<style scoped>
</style>
