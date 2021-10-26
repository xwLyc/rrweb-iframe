<template>
  <div class="flex psr">
    <div class="button-wrap">
      <button @click="startWeb">开始录制</button>
      <button @click="pauseWeb">结束录制</button>
      <button @click="playWeb">播放录制</button>
    </div>
    <div class="home flex1">
      <img alt="Vue logo" src="../assets/logo.png" />
      <HelloWorld msg="Welcome to Your Vue.js App" />
    </div>
    <div class="flex1">
      <iframe
        sandbox="allow-same-origin allow-scripts"
        src="http://localhost:3333"
        height="500"
        frameborder="0"
      ></iframe>
    </div>
  </div>
</template>

<script>
import { record } from 'rrweb'
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'
import rrwebPlayer from 'rrweb-player'
import 'rrweb-player/dist/style.css'

export default {
  name: 'Home',
  components: {
    HelloWorld
  },
  data() {
    return {
      homeEvent: [],
      homeRecord: null
    }
  },
  methods: {
    startWeb() {
      const _this = this
      this.homeRecord = record({
        emit(event) {
          // 用任意方式存储 event
          _this.homeEvent.push(event)
        }
      })
    },
    pauseWeb() {
      this.homeRecord && this.homeRecord()
    },
    playWeb() {
      new rrwebPlayer({
        target: document.body, // 可以自定义 DOM 元素
        // 配置项
        props: {
          events: this.homeEvent
        }
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