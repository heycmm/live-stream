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
    getQueryString() {
      var qs = location.search.substring(1), // 获取url中"?"符后的字串
          args = {}, // 保存参数数据的对象
          items = qs.length ? qs.split("&") : [], // 取得每一个参数项,
          item = null,
          len = items.length;

      for (var i = 0; i < len; i++) {
        item = items[i].split("=");
        var name = decodeURIComponent(item[0]),
            value = decodeURIComponent(item[1]);
        if (name) {
          args[name] = value;
        }
      }
      return args;
    },
  },
  mounted() {
    if (this.getQueryString['room']) {
      this.hlsUrl = this.hlsUrl + '/' + this.getQueryString['room'] + '/hls.m3u8'
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
