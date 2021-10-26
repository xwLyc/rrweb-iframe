<template>
  <div class="about">
    <h1>This is an about page</h1>
    <button @click="add">{{num}}</button>
  </div>
</template>
<script>
export default {
  data() {
    return {
      num: 1
    }
  },
  methods: {
    add() {
      this.num += 1
    }
  }
}
</script>