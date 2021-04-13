<template>
  <div class="hy-swiper">
    <div class="swiper" @transitionend="transitionend" @touchstart="touchStart" @touchmove="touchMove"
         @touchend="touchEnd">
      <slot></slot>
    </div>
  
  </div>
</template>

<script>
  import SwiperItem from "@/components/common/swiper/SwiperItem";
  
  export default {
    name: "Swiper",
    components: {SwiperItem},
    props: {
      showIndicator: {
        type: Boolean,
        default: true,
      }
    },
    data() {
      return {
        swiperStyle: {}, // swiper 样式
        swiperWidth: 0,
        slideCount: 0, // 元素个数
        currentIndex: 1, // 计数
      }
    },
    mounted() {
      setTimeout(() => {
        this.handleDom()
        // this.startTimer()
      }, 1000)
    },
    methods: {
      touchStart(e) {
        this.startX = e.targetTouches[0].pageX
      },
      touchMove(e) {
        this.moveX = e.targetTouches[0].pageX - this.startX
        this.setTransformX(-this.currentIndex * this.swiperWidth + this.moveX)
      },
      touchEnd(e) {
        if (Math.abs(this.moveX) > 50) {
          if (this.moveX > 0) {
            this.currentIndex--
          } else {
            this.currentIndex++
          }
        }
        this.swiperStyle.transition = '.3s'
        this.setTransformX(-this.currentIndex * this.swiperWidth)
      },
      transitionend() {
        if (this.currentIndex >= this.slideCount + 1) {
          this.currentIndex = 1
          this.swiperStyle.transition = 'none'
          this.setTransformX(-this.currentIndex * this.swiperWidth)
        } else if (this.currentIndex < 1) {
          this.currentIndex = 4
          this.swiperStyle.transition = 'none'
          this.setTransformX(-this.currentIndex * this.swiperWidth)
        }
      },
      setTransformX(position) {
        this.swiperStyle.transform = `translateX(${position}px)`
      },
      handleDom() {
        let swiperEl = document.querySelector('.swiper')
        let slidesEls = swiperEl.getElementsByClassName('slide')
        this.slideCount = slidesEls.length
        this.swiperStyle = swiperEl.style
        this.swiperWidth = swiperEl.offsetWidth
        if (this.slideCount > 1) {
          let cloneFirst = slidesEls[0].cloneNode(true)
          let cloneLast = slidesEls[this.slideCount - 1].cloneNode(true)
          swiperEl.insertBefore(cloneLast, slidesEls[0])
          swiperEl.appendChild(cloneFirst)
        }
        this.setTransformX(-this.currentIndex * this.swiperWidth)
      },
      startTimer() {
        this.playTimer = setInterval(() => {
          this.currentIndex++
          this.swiperStyle.transition = '.3s'
          this.setTransformX(-this.currentIndex * this.swiperWidth)
        }, 2000)
      }
    },
  }
</script>

<style scoped>
  .hy-swiper {
    position: relative;
    overflow: hidden;
  }
  
  .swiper {
    display: flex;
  }
  
  .indicator {
    display: flex;
    position: absolute;
    bottom: 10px;
    width: 100%;
    justify-content: center;
  }
  
  .indi-item {
    width: 8px;
    height: 8px;
    border-radius: 4px;
    background-color: #fff;
    margin: 0 5px;
  }
  
  .indi-item.active {
    background-color: rgba(212, 62, 46, 1.0);
  }
</style>