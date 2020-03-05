<template>
  <div class="v-scroll-item">
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: 'ScrollItem',
  inject: ['ScrollParent'],
  props: {
    // 触发条件top
    top: {
      type: Number,
      default: 0
    },
    // 是否一次性的
    once: {
      type: Boolean,
      default: false
    },
    // 子组件的接收方法名
    method: {
      type: String,
      default: 'animate'
    }
  },
  data () {
    return {
      triggered: false
    }
  },
  computed: {
    offset () {
      const ret = this.ScrollParent.offset + (this.$el ? this.$el.getBoundingClientRect().top : 0) - this.top - (this.ScrollParent.$el ? this.ScrollParent.$el.getBoundingClientRect().top : 0)
      return ret > 0 ? 0 : ret
    }
  },
  watch: {
    offset (v) {
      if (this.once && this.triggered) {
        return
      }
      this.triggered = true
      this.$children[0] && this.$children[0][this.method] && this.$children[0][this.method](v)
    }
  }
}
</script>
