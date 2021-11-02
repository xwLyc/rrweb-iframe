<script setup>
// This starter template is using Vue 3 <script setup> SFCs
// Check out https://v3.vuejs.org/api/sfc-script-setup.html#sfc-script-setup
import HelloWorld from './components/HelloWorld.vue'
import { record } from 'rrweb'

// rrweb.record({
//   emit(event) {
//     storagePush(event);
//   },
// });

window.childrenEvent = []
window.recordEvent = null
function startWeb() {
  childrenEvent = []
  recordEvent = record({
    recordCanvas: true,
    // keepIframeSrc: this.allowIframe,
    emit(event) {
      // 用任意方式存储 event
      // console.log('childrenEvent---------', event);
      childrenEvent.push(event)
    }
  })
}
function endWeb() {
  window.parent.postMessage(
    {
      type: 'endWeb',
      event: childrenEvent
    },
    '*'
  )
}

function saveWeb() {
  recordEvent && recordEvent()
  window.parent.postMessage(
    {
      type: 'saveWeb',
      event: childrenEvent
    },
    '*'
  )
}

window.addEventListener('message', function (event) {
  if (event.data === 'startWeb') {
    startWeb()
  }
  if (event.data === 'endWeb') {
    endWeb()
  }
  if (event.data === 'saveWeb') {
    saveWeb()
  }
})
</script>

<template>
  <img alt="Vue logo" src="./assets/logo.png" />
  <HelloWorld msg="Hello Vue 3 + Vite" />
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
