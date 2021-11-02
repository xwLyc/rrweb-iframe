<template>
  <div class="flex psr">
    <div class="button-wrap">
      <button @click="startWeb">开始录制</button>
      <button @click="saveWeb">保存录制</button>
    </div>
    <div class="home flex1">
      <img alt="Vue logo" src="../assets/logo.png" />
      <HelloWorld msg="Welcome to Your Vue.js App" />
    </div>
    <div class="flex1">
      <iframe id="authIframe" src="http://localhost:3333" height="500" frameborder="0"></iframe>
    </div>
  </div>
</template>

<script>
import { record } from 'rrweb'
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'
// import rrwebPlayer from 'rrweb-player'
// import { record } from '../rrweb-all'

export default {
  name: 'Home',
  components: {
    HelloWorld
  },
  data() {
    return {
      homeEvents: [],
      homeRecord: null,
      iframeEvents: []
    }
  },
  methods: {
    startWeb() {
      // 通知authIframe进行录制
      const authIframe = document.getElementById('authIframe')
      authIframe.contentWindow.postMessage('startWeb', '*')

      // 开始录制
      const _this = this
      this.homeRecord = record({
        emit(event) {
          // 用任意方式存储 event
          _this.homeEvents.push(event)
        }
      })
    },
    saveWeb() {
      // 通知authIframe停止录制，保存录制数据
      const authIframe = document.getElementById('authIframe')
      authIframe.contentWindow.postMessage('saveWeb', '*')

      // 停止录制
      this.homeRecord && this.homeRecord()

      // 存储数据
      localStorage.setItem(
        'allEvents',
        JSON.stringify({
          homeEvents: this.homeEvents,
          iframeEvents: this.iframeEvents
        })
      )

      window.addEventListener('message', event => {
        console.log('-----children event ----', event.data.event)
        console.log('-----parent event ----', this.homeEvents)
        if (event.data.type === 'saveWeb') {
          this.iframeEvents = event.data.event
        }

        localStorage.setItem(
          'allEvents',
          JSON.stringify({
            homeEvents: this.homeEvents,
            iframeEvents: this.iframeEvents
          })
        )
      })
    }
  }
}
</script>

<style lang="scss" scoped>
.flex {
  display: flex;
}
.flex1 {
  flex: 1;
}
.psr {
  position: relative;
}
.button-wrap {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  margin: auto;
  z-index: 1;
  button {
    margin: 0 5px;
  }
}
</style>
