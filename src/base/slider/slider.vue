<template>
  <div class="slider" ref="slider">
    <div class="slider-group" ref="sliderGroup">
      <!-- slot可以将组件内部的dom插入到这里 -->
      <slot>
      </slot>
    </div>
    <div class="dots">
      <span class="dot" v-for="item in dots" :key="item"></span>
    </div>
  </div>
</template>
<script type="text/ecmascript-6">
import BScroll from 'better-scroll'
import { addClass } from 'common/js/dom'
export default {
  name: 'slider',
  directives: {},
  components: {},
  data() {
    return {
      dots: []
    }
  },
  props: {
    loop: {
      type: Boolean,
      default: true
    },
    autoPlay: {
      type: Boolean,
      default: true
    },
    interval: {
      type: Number,
      default: 4000
    }
  },
  computed: {},
  watch: {},
  methods: {
    _setSliderWidth() {
      // 获取所有ref为sliderGroup结点下的子节点
      this.children = this.$refs.sliderGroup.children

      let width = 0
      let sliderWidth = this.$refs.slider.clientWidth
      console.log(this.$refs.slider)
      console.log(sliderWidth)
      console.log(this.children.length)
      for (let i = 0; i < this.children.length; i++) {
        let child = this.children[i]
        // 为所有ref为sliderGroup结点下的子节点添加slider-item样式
        addClass(child, 'slider-item')

        child.style.width = sliderWidth + 'px'
        width += sliderWidth
      }

      // 如过循环滚动，初始化宽度需要*2
      if (this.loop) {
        width += 2 * sliderWidth
      }
      console.log(this.$refs)
      this.$refs.sliderGroup.style.width = width + 'px'
    },
    _initSlider() {
      this.slider = new BScroll(this.$refs.slider, {
        scrollX: true,
        scrollY: false,
        // 惯性
        momentum: false,
        snap: true,
        snapLoop: this.loop,
        snapThreshold: 0.3,
        snapSpeed: 400,
        click: true
      })
    },
    _initDots() {
      this.dots = new Array(this.children.length)
    }
  },
  beforeCreate() {},
  created() {},
  destroyed() {},
  mounted() {
    // 浏览器的刷新时间17毫秒
    setTimeout(() => {
      this._setSliderWidth()
      // 需要在slider初始化前设置dots，否则dots个数会出错
      this._initDots()
      this._initSlider()
    }, 20)
  }
}
</script>
<style scoped lang="stylus" rel="stylesheet/stylus">
@import '~common/stylus/variable';

.slider {
  min-height: 1px;

  .slider-group {
    position: relative;
    overflow: hidden;
    white-space: nowrap;

    .slider-item {
      float: left;
      box-sizing: border-box;
      overflow: hidden;
      text-align: center;

      a {
        display: block;
        width: 100%;
        overflow: hidden;
        text-decoration: none;
      }

      img {
        display: block;
        width: 100%;
      }
    }
  }

  .dots {
    position: absolute;
    right: 0;
    left: 0;
    bottom: 12px;
    text-align: center;
    font-size: 0;

    .dot {
      display: inline-block;
      margin: 0 4px;
      width: 8px;
      height: 8px;
      border-radius: 50%;
      background: $color-text-l;

      &.active {
        width: 20px;
        border-radius: 5px;
        background: $color-text-ll;
      }
    }
  }
}
</style>