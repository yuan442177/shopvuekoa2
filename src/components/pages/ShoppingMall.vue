<template>
    <div>
        <div class="search-bar">
            <van-row>
                <van-col span="3">
                    <img :src="locationIcon" width="80%" class="location-icon">
                </van-col>
                <van-col span="16">
                    <input type="text" class="search-input" >
                </van-col>
                <van-col span="5">
                    <van-button size="mini">查找</van-button>
                </van-col>
            </van-row>
        </div>
        <!--swipwer area-->
        <div class="swiper-area">
            <van-swipe :autoplay="1000">
                <van-swipe-item v-for="(banner,index) in bannerPicArray" :key="index">
                    <img v-lazy="banner.image" width="100%"/>
                </van-swipe-item>
            </van-swipe>
        </div>
        <!--type-bar-->
        <div class="type-bar">
            <div  v-for="(cate,index) in category" :key="index" >
                    <img v-lazy="cate.image" width="90%" />
                    <span>{{cate.mallCategoryName}}</span> 
            </div>
        </div>
        <!--AD banner area-->
        <div class="ad-banner">
            <img v-lazy="adBanner.PICTURE_ADDRESS" width="100%">
        </div>
        <!--Recommend goods area-->
        <div class="recommend-area">
            <div class="recommend-title">
                商品推荐
            </div>
            <div class="recommend-body">
                <!--swiper-->
                <swiper :options="swiperOption">
                    <swiper-slide v-for=" (item ,index) in recommendGoods" :key="index">
                        <div class="recommend-item">
                                <img :src="item.image" width="80%" />
                                <div>{{item.goodsName}}</div>
                                <div>￥{{item.price}} (￥{{item.mallPrice}})</div>   
                        </div>
                    </swiper-slide>
                </swiper>
            </div>
        </div>
    </div>
</template>

<script>
    import axios from 'axios'
    import 'swiper/dist/css/swiper.css'
    import { swiper, swiperSlide } from 'vue-awesome-swiper'

    export default {
        data() {
            return {
            locationIcon: require('../../assets/images/location.png'),
            bannerPicArray:[],
            category:[],
            adBanner:[],
            recommendGoods:[],
            swiperOption:{
                slidesPerView:3
            }
            }
        },
        components: {
                swiper,
                swiperSlide
        },
        created(){
            axios({
                url:'https://www.easy-mock.com/mock/5d316bba2a5bf87ce5f30b0c/shopvue/index',
                method:'get',
            })
            .then(response=>{
                console.log(response)
                if(response.status==200){
                this.category=response.data.data.category
                this.adBanner = response.data.data.advertesPicture //获得广告图片
                this.bannerPicArray = response.data.data.slides   //轮播图片
                this.recommendGoods = response.data.data.recommend  //推荐商品
                }
            })
            .catch(error=>{
                console.log(error)
            })
        }
    }
</script>

<style scoped>
    .search-bar{
        height: 2.2rem;
        background-color: #e5017d;
        line-height: 2.2rem;
        overflow: hidden;
    }
    .search-input{
        width: 100%;
        height: 1.3rem;
        border-top: 0px;
        border-left: 0px;
        border-right: 0px;
        border-bottom: 1px solid #fff !important;
        background-color: #e5017d;
        color: #fff;
    }
    .location-icon{
        padding-top: 0.2rem;
        padding-left: 0.3rem;
    }
    .swiper-area{
      width:20rem;
      clear: both;
      overflow: hidden;
    }
     .type-bar{
      background-color: #fff;
      margin:0 .3rem .3rem .3rem;
      border-radius: .3rem;
      font-size:14px;
      display: flex;
      flex-direction:row;
      flex-wrap:nowrap;
    }
  .type-bar div{
      padding: .3rem;
      font-size: 12px;
      text-align: center;
    }
     .recommend-area{
       background-color: #fff;
       margin-top: .3rem;
    }
    .recommend-title{
        border-bottom:1px solid #eee;
        font-size:14px;
        padding:.2rem;
        color:#e5017d;
    }
      .recommend-body{
       border-bottom: 1px solid #eee;
   }
  
    .recommend-item{
        width:99%;
        border-right: 1px solid #eee;
        font-size: 12px;
        text-align: center;
    }
 
</style>