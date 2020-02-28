<template>
  <div class="cnt">
    <Header></Header>
    <web>
      <game></game>
    </web>
  </div>
</template>

<script>
  import Vue from 'vue'
  import Header from '../common/Header.vue'
  import Footer from '../common/Footer.vue'
  import Game from '../common/Game.vue'
  import Web from 'reduce-loader!../common/Web.vue'
  import 'reduce-loader!./web'

  export default Vue.extend({
    name: 'Home',
    components: {
      Header,
      Footer,
      Web,
      Game
    },
    created() {
      window.addEventListener('wxload', query => console.log('page1 wxload', query))
      window.addEventListener('wxshow', () => console.log('page1 wxshow'))
      window.addEventListener('wxready', () => console.log('page1 wxready'))
      window.addEventListener('wxhide', () => console.log('page1 wxhide'))
      window.addEventListener('wxunload', () => console.log('page1 wxunload'))

      if (process.env.isMiniprogram) {
        console.log('I am in miniprogram')
      } else {
        console.log('I am in Web')
      }

    },
    mounted: function () {

    },
    data() {
      return {}
    },
    methods: {

    },
  })
</script>

<style lang="less">

</style>