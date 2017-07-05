<template>
  <div class="ratings" ref="ratings">
    <div class="ratings-content">
      <div class="overview">
        <div class="overview-left">
          <h1 class="score">{{seller.score}}</h1>
          <div class="title">综合评分</div>
          <div class="rank">高于周边商家{{seller.rankRate}}</div>
        </div>
        <div class="overview-right">
          <div class="score-wrapper">
            <span class="title">服务态度</span>
            <star :size="36" :score="seller.serviceScore"></star>
            <span class="score">{{seller.serviceScore}}</span>
          </div>
          <div class="score-wrapper">
            <span class="title">商品评分</span>
            <star :size="36" :score="seller.foodScore"></star>
            <span class="score">{{seller.foodScore}}</span>
          </div>
          <div class="delivery-wrapper">
            <span class="title">送达时间</span>
            <span class="delivery">{{seller.deliveryTime}}分钟</span>
          </div>
        </div>
      </div>
      <split></split>
      <ratingselect :select-type="selectType" :only-content="onlyContent" :desc="desc" :ratings="ratings"></ratingselect>
      <div class="rating-wrapper" >
        <ul>
          <li v-for="rating in ratings" class="rating-item">
            <div class="avatar">
              <img width="28" height="28" :src="rating.avatar">
            </div>
            <div class="content">
              <h1 class="name">{{rating.username}}</h1>
              <div class="star-wrapper">
                <star :size="24" :score="rating.score"></star>
                <span class="delivery" v-show="rating.deliveryTime">{{rating.deliveryTime}}</span>
              </div>
              <p class="text">{{rating.text}}</p>
              <div class="recommend" v-show="rating.recommend.length">
                <span class="icon-thumb_up">@</span>
                <span v-for="item in rating.recommend" class="item">{{item}}</span>
              </div>
              <div class="time">
                {{rating.rateTime | formatDate}}
              </div>
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import star from '../star/star';
  import BScroll from 'better-scroll';
  import ratingselect from '../ratingselect/ratingselect';
  import {formatDate} from '../../common/js/date';
  import split from '../split/split';
  const ALL = 2;
  const ERR_OK = 0;
  export default {
    props: {
      seller: {
        type: Object
      },
      food: {
        type: Object
      }
    },
    data() {
      return {
        ratings: [],
        selectType: ALL,
        onlyContent: true,
        desc: {
          all: '全部',
          positive: '推荐',
          negative: '吐槽'
        }
      };
    },
    filters: {
      formatDate(time) {
        let date = new Date(time);
        return formatDate(date, 'yyyy-MM-dd hh:mm');
      }
    },
    created() {
      this.$http.get('/api/ratings').then((response) => {
        response = response.body;
        console.log(response.data);
        if (response.errno === ERR_OK) {
          this.ratings = response.data;
          this.$nextTick(() => {
              this.scroll = new BScroll(this.$refs.ratings, {
                click: true
              });
          });
        };
      });
    },
    components: {
      star,
      ratingselect,
      split
    }
  };
</script>

<style>
  .ratings{
    position:absolute;
    top:174px;
    bottom:0;
    left:0;
    width:100%;
    overflow:hidden;
  }
  .ratings .overview{
    display:flex;
    padding:18px 0;

  }
  .overview-left{
    flex:0 0 137px;
    padding:6px 0;
    width:137px;
    border-right:1px solid rgba(7,17,27,0.2);
    text-align:center;
  }
  .overview-left>.score{
    line-height:28px;
    margin-bottom:6px;
    font-size:24px;
    color:rgb(255,153,0);
  }
  .overview-left>.title{
    line-height:12px;
    margin-bottom:8px;
    font-size:12px;
    padding-top:6px;
    color:rgb(7,17,27);
  }
  .overview-left>.rank{
    line-height:10px;
    font-size:10px;
    padding-top:6px;
    color:rgb(147,153,159);
  }
  .overview-right{
    flex:1;
    padding: 6px 0 6px 24px;

  }
  .score-wrapper{
    margin-bottom:8px;
    line-height:18px;
    font-size:0;
  }
  .score-wrapper>.title{
    display:inline-block;
    vertical-align:top;
    line-height:10px;
    font-size:12px;
    color:rgb(7,17,27);
  }
  .score-wrapper>.star{
    display:inline-block;
    padding:0 12px;
    vertical-align:top;
  }
  .score-wrapper>.score{
    display:inline-block;
    vertical-align:top;
    line-height:18px;
    font-size:12px;
    color:rgb(255,153,0);
  }
  .delivery-wrapper{
    font-size:0;
  }
  .delivery-wrapper>.title{
    line-height:10px;
    font-size:12px;
    color:rgb(7,17,27);
  }
  .delivery-wrapper>.delivery{
    margin-left:12px;
    font-size:12px;
    color:rgb(147,153,159);
  }
  @media (max-width:320px){
    .overview-left{
      flex:0 0 100px;
      width:100px;
    }
    .star{
      padding:0 5px!important;
    }
    .star-item{
      margin-right:5px!important;
    }
  }
  @media (max-width:375px) and (min-width:321px){
    .overview-left{
      flex:0 0 120px;
      width:120px;
    }
    .star{
      padding:0 5px!important;
    }
    .star-item{
      margin-right:5px!important;
    }
  }
  .rating-wrapper{
    padding:0 18px;

  }
  .rating-wrapper .rating-item{
    display:flex;
    padding:18px 0;
    border-bottom:1px solid rgba(7,17,27,0.1);
  }
  .rating-wrapper .rating-item .avatar{
    flex:0 0 28px;
    width:28px;
    margin-right:12px;
  }
  .rating-wrapper .rating-item img{
    border-radius:50%;
  }
  .rating-wrapper .rating-item .content{
    position:relative;
    flex:1;
  }
  .rating-wrapper .rating-item .name{
    margin-bottom:4px;
    line-height:12px;
    font-size:10px;
    color:rgb(7,17,27);
  }
  .rating-wrapper .rating-item .star-wrapper{
    margin-bottom:6px;
    font-size:0;
    text-align:left;
    margin-top:0;
    padding:0;
  }
  .rating-wrapper .rating-item .star{
    margin-right:6px;
    display:inline-block;
    vertical-align:top;
  }
  .rating-wrapper .rating-item .delivery{
    margin-bottom:4px;
    display:inline-block;
    line-height:12px;
    font-size:10px;
    color:rgb(147,153,159);
  }
  .rating-wrapper .rating-item .text{
    margin-bottom:8px;
    line-height:18px;
    color:rgb(7,17,27);
    font-size:12px;
  }
  .rating-wrapper .rating-item .recommend{
    line-height:16px;
    margin-bottom:5px;
  }
  .rating-wrapper .rating-item .icon-thumb_up{
    display:inline-block;
    font-size:10px;
    width:18px;
    height:18px;
    text-align:center;
    background:rgb(0,160,220);
    border-radius:50%;
    color:#fff;
  }
  .rating-wrapper .rating-item .item{
    padding:0 6px;
    border:1px solid rgba(7,17,27,0.1);
    border-radius:1px;
    color:rgb(147,153,159);
    background:#fff;
    font-size:12px;
  }
  .rating-wrapper .rating-item .time{
    position:absolute;
    line-height:12px;
    top:0;
    font-size:10px;
    right:0;
  }
</style>
