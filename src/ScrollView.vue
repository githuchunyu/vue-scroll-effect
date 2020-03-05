<template>
  <div class="v-scroll-view" :style="{ transform }">
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: 'ScrollView',
  props: {
    inertia: {
      type: Number,
      default: 20
    },
    speed: {
      type: Number,
      default: 0.5
    },
    top: {
      type: Number,
      default: 0
    },
    bottom: {
      type: Number,
      default: 0
    },
    disabled: {
      type: Boolean,
      default: false
    }
  },
  data () {
    return {
      delta: 0,
      offset: 0
    }
  },
  computed: {
    transform () {
      return `translateY(${this.offset}px)`
    }
  },
  provide () {
    return { ScrollParent: this }
  },
  mounted () {
    window.addEventListener('mousewheel', this.handleWheel)
    // 是否需要禁止回弹
    window.addEventListener('mousewheel', e => { 
      e.preventDefault()
    }, { passive: false })
  },
  beforeDestroy () {
    window.removeEventListener('mousewheel', this.handleWheel)
  },
  methods: {
    animate () {
      cancelAnimationFrame(this.timer)
      this.timer= requestAnimationFrame(() => {
        // 计算实际偏差
        let offset = this.offset + (this.delta - this.offset) / this.inertia
        if (Math.abs(offset - this.delta) < 0.02) {
          offset = this.delta
        }
        offset = Math.ceil(offset)
        this.offset = offset
        this.$emit('on-scroll', offset)
        if (offset !== this.delta) {
          this.animate()
        }
      })
    },
    handleWheel (e) {
      if (this.disabled) {
        return
      }
      const delta = this.delta + e.wheelDeltaY * this.speed
      const height = -1 * this.$el.getBoundingClientRect().height + document.documentElement.clientHeight - this.bottom
      this.delta = Math.max(height, Math.min(this.top, delta))
      this.animate()
    }
  }
}
</script>

<style lang="css">
.v-scroll-view {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 100;
  will-change: transform;
}
</style>
