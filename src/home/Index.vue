<template>
  <div class="cnt">
    <Header></Header>
    <web>
      <game></game>
    </web>
    <Footer></Footer>
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
      this.min_setup = 0;
    },
    mounted: function () {
      this.hanoi(3, "A", "B", "C");
    },
    data() {
      return {
        min_setup: 0,
      }
    },
    methods: {
      hanoi(n, a, b, c) {
        this.min_setup++;
        if (n == 1) {
          console.log("Move " + n + " from " + a + " to " + c);
        } else {
          this.hanoi(n - 1, a, c, b);
          console.log("Move " + n + " from " + a + " to " + c)
          this.hanoi(n - 1, b, a, c);
        }
      }
    },
  })
</script>

<style lang="less">

</style>