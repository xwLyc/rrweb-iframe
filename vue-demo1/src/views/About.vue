<template>
  <div class="about">
    <button @click="playWeb">开始回放</button>
    <div id="replaycontent" style="width: 100%;height: 900px;"></div>
  </div>
</template>
<script>
import { Replayer } from '../rrweb-all'
import 'rrweb-player/dist/style.css'

export default {
  data() {
    return {
      Replayer,
      homeReplayer: null,
      iframeReplayer: null,
      replayContent: null
    }
  },
  mounted() {
    this.initPlayWeb()
  },
  methods: {
    initPlayWeb() {
      const { homeEvents, iframeEvents } = JSON.parse(localStorage.getItem('allEvents'))
      console.log('homeEvents', homeEvents)
      console.log('iframeEvents', iframeEvents)

      this.homeReplayer = new this.Replayer(homeEvents, {
        root: document.getElementById('replaycontent'),
        iframesPlayer: [
          {
            iframeId: 'authIframe',
            iframeEvents: iframeEvents
          }
        ],
        UNSAFE_replayCanvas: true
      })
      this.homeReplayer.play()
    },
    playWeb() {
      this.homeReplayer.play()
    }
  }
}
</script>
<style>
.replayer-wrapper {
  position: inherit;
}
</style>
