<template>
  <view>
    <!-- 轮播图 -->
     <swiper :indicator-dots="true" :autoplay="true" :interval="3000" :duration="1000" class="swiper">
       <swiper-item v-for="(item,i) in swiperList" :key="i">
         <navigator :url="'/subpck/goodsDetail/goodsDetail?goods_id='+item.goods_id">
          <image :src="item.image_src" class="swiper-img"></image>
        </navigator>
       </swiper-item>
     </swiper>
     <!-- 分类 -->
     <view class="nav-list">
         <view v-for="(item,i) in navList" :key="i" class="nav-item" @click="navHanlder(item)">
           <image :src="item.image_src" class="nav-img"></image>
         </view>
     </view>
  </view>
</template>

<script>
  export default {
    data() {
      return {
            swiperList: [],
            navList:[],
      };
    },
   onLoad() {
      this.getSwiperList()
      this.getNavList()
    },
    methods:{
       // 获取轮播图数据的方法
          async getSwiperList() {
            // 1 发起请求
            const { data: res } = await uni.$http.get('/api/public/v1/home/swiperdata')
            // 2 请求失败
            if (res.meta.status !== 200) {
              return uni.$showMsg()
            }
            // 3 请求成功，为 data 中的数据赋值
            this.swiperList = res.message
          },
      //获取分类数据
      async getNavList() {
         const { data: res } =  await uni.$http.get('/api/public/v1/home/catitems')
         if(res.meta.status !=200) {
           return uni.$showMsg()
         }
         this.navList = res.message
      },
     //分类事件
     navHanlder(item){
       if(item.name ==="分类"){
         uni.switchTab({
           url:'/pages/cate/cate'
         })
       }
     }
    }
  }
</script>

<style lang="scss">
  .swiper{
    height:330rpx;
  }
.swiper-img{
  width:100%;
}
.nav-list{
  display: flex;
  justify-content: space-around;
  margin:30rpx 0;
  .nav-img {
      width: 128rpx;
      height: 140rpx;
    }
  
}
</style>
