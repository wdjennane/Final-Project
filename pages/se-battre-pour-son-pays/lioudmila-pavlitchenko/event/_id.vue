<template>
  <section>
    <RightContent v-if="page.id === '1'" :page="page" />
    <LeftContent v-if="page.id === '2.1'" :page="page" />
    <Video v-if="page.video" :page="page" />
  </section>
</template>

<script>
import RightContent from '../../../../components/RightContent'
import LeftContent from '../../../../components/LeftContent'
import Video from '../../../../components/Video'

const data = require('./event.json')

export default {
  transition: {
    name: 'slide',
    duration: 500
  },
  components: {
    RightContent,
    LeftContent,
    Video
  },
  data: function () {
    return {
      page: data.find(p => p.id === this.$route.params.id),
      canScroll: false
    }
  },
  beforeMount() {
    setTimeout(() => {
      this.canScroll = true
    }, 1500)
    window.addEventListener('wheel', this.onScroll)
  },
  beforeDestroy() {
    window.removeEventListener('wheel', this.onScroll)
  },
  methods: {
    onScroll() {
      if (this.canScroll === true) {
        if (event.deltaY < 0) {
          if (this.page.prev) {
            this.$router.push({ path: this.page.prev })
          }
        }
        if (event.deltaY > 0) {
          if (this.page.next) {
            this.$router.push({ path: this.page.next })
          }
        }
      }
    }
  }
}
</script>
