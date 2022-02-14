<template>
    <vue-plyr class="container" ref="plyr" :options="playerOptions">
        <video ref="movie" data-plyr-config="{'autoplay': false}"></video>
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
                controls: ['play-large', 'play', 'progress', 'current-time', 'mute', 'volume', 'settings', 'fullscreen'],
                settings: ['quality', 'speed', 'loop'],
            }
        }
    },
    computed: {
        player() {
            return this.$refs.plyr.player;
        },
    },
    mounted() {
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