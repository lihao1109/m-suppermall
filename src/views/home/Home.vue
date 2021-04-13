<template>
  <div>
    <nav-bar class="home-nav">
      <div slot="center">购物街</div>
    </nav-bar>
    <swiper>
      <swiper-item v-if="banners" v-for="item in banners">
        <a href="#"><img :src="item.image" alt=""></a>
      </swiper-item>
    </swiper>
  </div>
</template>

<script>
  import NavBar from "@/components/common/navbar/NavBar";
  import {Swiper, SwiperItem} from '@/components/common/swiper'
  
  import {getHomeMultidata} from "@/network/home";
  
  export default {
    name: "Home",
    props: {
      showIndicator: true
    },
    data() {
      return {
        banners: [],
        recommends: [],
      }
    },
    components: {
      NavBar,
      Swiper,
      SwiperItem
    },
    created() {
      // 1. 请求多个数据
      getHomeMultidata().then(res => {
        // console.log(res)
        this.banners = res.data.banner.list;
        this.recommends = res.data.recommend.list;
      })
    },
  }
</script>

<style scoped>
  .home-nav {
    background-color: var(--color-tint);
    color: #fff;
  }
</style>
