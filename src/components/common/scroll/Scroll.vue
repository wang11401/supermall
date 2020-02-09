<template>
  <div class="wrapper" ref="wrapper">
    <div class="content">
      <slot></slot>
    </div>
  </div>
</template>

<script>
  import BScroll from 'better-scroll'
  export default {
    name: "Scroll",
    props: {
      probeType: {
        type: Number,
        default: 0
      },
      pullUpLoad: {
        type: Boolean,
        default: false
      }
    },
    data() {
      return {
        bScroll: null
      }
    },
    mounted() {
      this.bScroll = new BScroll(this.$refs.wrapper, {
        click: true,
        probeType: this.probeType,
        pullUpLoad: this.pullUpLoad
      })

      if(this.probeType ===2 || this.probeType ===3) {
        this.bScroll.on('scroll', position => {
          // console.log(position);
          this.$emit('scroll', position)
        })
      }
      if(this.pullUpLoad) {
        this.bScroll.on('pullingUp', () => {
          this.$emit('pullingUp')
        })
      }
    },
    methods: {
      scrollTo(x, y, time = 500) {
        this.bScroll && this.bScroll.scrollTo(x, y, time)
      },
      finishPullUp() {
        this.bScroll && this.bScroll.finishPullUp()
      },
      refresh() {
        // console.log('------');
        this.bScroll && this.bScroll.refresh()
      },
      getScrollY() {
        return this.bScroll ? this.bScroll.y : 0
      }
    }
  }
</script>

<style scoped>

</style>
