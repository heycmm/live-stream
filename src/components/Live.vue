<template>
  <vue-plyr class="container" ref="plyr" :options="playerOptions">
    <video></video>
  </vue-plyr>
</template>

<script>
import Hls from 'hls.js'
import VuePlyr from 'vue-plyr'
import 'vue-plyr/dist/vue-plyr.css'

export default {
  name: 'Live',
  components: {
    VuePlyr
  },
  data() {
    return {
      hlsUrl: process.env.VUE_APP_HLS_URL,
      playerOptions: {
        controls: ['play-large', 'play', 'mute', 'volume', 'fullscreen'],
        settings: ['quality', 'speed', 'loop'],
      }
    }
  },
  computed: {
    player() {
      return this.$refs.plyr.player;
    },
    GetUrlRelativePath() {
      const url = document.location.toString();
      const arrUrl = url.split("//");
      const start = arrUrl[1].indexOf("/");
      let relUrl = arrUrl[1].substring(start);
      if (relUrl.indexOf("?") !== -1) {
        relUrl = relUrl.split("?")[0];
      }
      return relUrl;
    }
  },
  mounted() {
    if (this.GetUrlRelativePath){
      this.hlsUrl=this.hlsUrl+this.GetUrlRelativePath+'/hls.m3u8'
    }
    if (Hls.isSupported()) {
      const hls = new Hls();
      hls.loadSource(this.hlsUrl);
      hls.attachMedia(this.player.media);
      window.hls = hls;
    }
  },

}
</script>
<style scoped>
</style>
