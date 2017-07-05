<template>
  <div class="seller" ref="seller">
    <div class="seller-content">
      <div class="overview">
        <h1 class="title">{{seller.name}}</h1>
        <div class="desc">
          <star :size="36" :score="seller.score"></star>
          <span class="text">({{seller.ratingCount}})</span>
          <span class="text">月售{{seller.sellCount}}单</span>
        </div>
        <ul class="remark">
          <li class="block">
            <h2>超送价</h2>
            <div class="content">
              <span class="stress">{{seller.minPrice}}元</span>
             </div>
          </li>
          <li class="block">
            <h2>商家配送</h2>
            <div class="content">
              <span class="stress">{{seller.deliveryPrice}}元</span>
             </div>
          </li>
          <li class="block">
            <h2>平均配送时间</h2>
            <div class="content">
              <span class="stress">{{seller.deliveryTime}}分钟</span>
             </div>
          </li>
        </ul>
        <div class="favorite" >
          <span class="icon-favorite" @click="toggleFavorite" :class="{'active':toggleFavorite===true}">❤</span>
          <span class="text">{{favoriteText}}</span>
        </div>
      </div>
      <split></split>
      <div class="bulletin">
        <h1 class="title">公告与活动</h1>
        <div class="content-wrapper">
          <p class="content">{{seller.bulletin}}</p>
        </div>
        <ul v-if="seller.supports" class="supports">
          <li class="support-item" v-for="item in seller.supports">
            <span class="icon" :class="classMap[item.type]"></span>
            <span class="text">{{item.description}}</span>
          </li>
        </ul>
      </div>
      <split></split>
      <div class="pics">
        <h1 class="title">商家实景</h1>
        <div class="pic-wrapper" ref="picWrapper">
          <ul class="pic-list" ref="picList">
            <li class="pic-item" v-for="pic in seller.pics">
              <img width="120" height="90" :src="pic">
            </li>
          </ul>
        </div>
      </div>
      <split></split>
      <div class="info">
        <h1 class="title">商家信息</h1>
        <ul>
          <li class="info-item" v-for="info in seller.infos">
            {{info}}
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import star from '../star/star';
  import split from '../split/split';
  import BScroll from 'better-scroll';
  export default {
    props: {
      seller: {
        type: Object
      }
    },
    data() {
      return {
        favorite: false
      };
    },
    computed: {
      favoriteText() {
        return this.favorite === true ? '已收藏' : '收藏';
      }
    },
    created() {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    },
    mounted() {
      this._initScroll();
      this._initPics();
    },
    watch: {
      'seller'() {
        this._initScroll();
        this._initPics();
      }
    },
    methods: {
      _initScroll() {
        this.$nextTick(() => {
            this.scroll = new BScroll(this.$refs.seller, {
              click: true
            });
        });
      },
      _initPics() {
        if (this.seller.pics) {
          console.log(1);
          let picWidth = 120;
          let margin = 6;
          let width = (picWidth + margin) * this.seller.pics.length - margin;
          this.$refs.picList.style.width = width + 'px';
          console.log(this.$refs.picList.style.width);
          this.$nextTick(() => {
            if (!this.picScroll) {
              this.picScroll = new BScroll(this.$refs.picWrapper, {
                scrollX: true,
                eventPassthrough: 'vertical'
              });
            } else {
              this.picScroll.refresh();
            }
          });
        }
      },
      toggleFavorite(event) {
        if (!event._constructed) {
          return;
        }
        console.log(this.favorite);
        this.favorite = !this.favorite;
      }
    },
    components: {
      star,
      split
    }
  };
</script>

<style>
  .seller{
    position:absolute;
    left:0;
    bottom:0;
    top:174px;
    width:100%;

    overflow:hidden;
  }
  .seller .overview{
    padding:18px;
  }
  .seller .overview .title{
    margin-bottom:8px;
    line-height:14px;
    color:rgb(7,17,27);
    font-size:14px;
  }
  .seller .overview .desc{
    borer-bottom:1px solid rgba(7,17,27,0.1);
    padding-bottom:18px;

    font-size:0;
  }
  .seller .overview .star{
    display:inline-block;
    margin-right:8px;
    vertical-align:top;
  }
  .seller .overview .text{
    margin-right:12px;
    display:inline-block;
    line-height:18px;
    vertical-align:top;
    font-size:10px;
    color:rgb(77,85,93);

  }
  .seller .overview .remark{
    display:flex;
    padding-top:18px;

  }
  .seller .overview .favorite{
    position:absolute;
    right:18px;
    top:18px;
    text-align:center;
  }
  .seller .overview .icon-favorite{
    display:block;
    color:#d4d6d9;
    margin-bottom:4px;
    line-height:24px;
    font-size:24px;

  }
  .seller .overview .favorite .text{
    display:inline-block;
    padding-left:8px;
    text-align:center;
  }
  .seller .overview .icon-favorite.active{
    color:rgb(240,20,20);
  }
  .seller .overview .text{
    line-height:10px;
  }
  .seller .overview .block{
    flex:1;
    text-align:center;
    border-right:1px solid rgba(7,17,27,0.1);
  }
  .seller .overview .block:last-child{
    border:none;
  }
  .seller .overview h2{
    margin-bottom:4px;
    line-height:10px;
    font-size:10px;
    color:rgb(147,153,159);
  }
  .seller .overview .content{
    line-height:24px;
    font-size:10px;
    color:rgb(7,17,27);
  }
  .seller .overview .stress{
    font-size:24px;
  }
  .seller .bulletin{
    padding:18px 18px 0 18px;

  }
  .seller .bulletin .title{
    margin-bottom:8px;
    line-height:14px;
    color:rgb(7,17,27);
    font-size:14px;
  }
  .seller .bulletin .content-wrapper{
    padding:16px 12px;
    border-bottom:1px solid rgba(7,17,27,0.1);
  }
  .seller .bulletin .content-wrapper .content{
    line-height:24px;
    font-size:12px;
    color:rgb(240,20,20);
  }
  .seller .bulletin .supports .support-item{
    padding:16px 12px;
    font-size:0;
    border-bottom:1px solid rgba(7,17,27,0.1);
  }
  .supports .icon{
    display:inline-block;
    width:12px;
    height:12px;
    margin-right:4px;
    background-size:12px;
    background-repeat:no-repeat;
    vertical-align:top;
  }
  .decrease{
    background-image:url('decrease_4@2x.png');
  }
  .discount{
    background-image:url('discount_4@2x.png');
  }
  .guarantee{
    background-image:url('guarantee_4@2x.png');
  }
  .invoice{
    background-image:url('invoice_4@2x.png');
  }
  .special{
    background-image:url('special_4@2x.png');
  }
  .supports .text{
    line-height:16px;
    font-size:12px;
    color:rgb(7,17,27);
  }
  .seller .pics{
    padding:18px;
  }
  .seller .pics .title{
    margin-bottom:12px;
    line-height:14px;
    color:rgb(7,17,27);
    font-size:14px;
  }
  .seller .pics .pic-wrapper{
    width:100%;
    overflow:hidden;
    white-space:nowrap;

  }
  .seller .pics .pic-list{
    font-size:0;

  }
  .seller .pics .pic-list .pic-item{
    display:inline-block;
    margin-right:6px;
    width:120px;
    height:90px;
  }
  .seller .pics .pic-list .pic-item:last-child{
    margin:0;
  }
  .seller .info{
    padding:18px 18px 0 18px;
  }
  .seller .info .title{
    padding-bottom:12px;
    line-height:14px;
    color:rgb(7,17,27);
    font-size:14px;
    border-bottom:1px solid rgba(7,17,27,0.1);
  }
  .seller .info .info-item{
    padding:16px 12px;
    line-height:16px;
    color:rgb(7,17,27);
    font-size:12px;
    border-bottom:1px solid rgba(7,17,27,0.1);
  }
  .seller .info .info-item:last-child{
    border:none;
  }
</style>
