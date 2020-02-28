<template>
  <div class="header">
    <p>{{headerTips}} {{input}}</p>
  </div>
</template>

<script>
  import {
    mapState,
    mapActions
  } from 'vuex'

  export default {
    name: 'Header',
    computed: {
      ...mapState(['headerTips', 'input']),
    },
    mounted() {
      this.FAKE_ACTION('请将数字移到第三个盘上，小的在上')
    },
    methods: {
      ...mapActions(['FAKE_ACTION']),
    },
  }
</script>

<style lang="less">
  .header {
    margin-bottom: 10px;
    width: 100%;
    text-align: center;
  }
</style>