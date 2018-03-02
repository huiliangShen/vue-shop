<template>
  <div class="ratings" v-el:ratingsw>
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
            <v-star :size="36" :score="seller.serviceScore"></v-star>
            <span class="score">{{seller.serviceScore}}</span>
          </div>
          <div class="score-wrapper">
            <span class="title">商品评分</span>
            <v-star :size="36" :score="seller.foodScore"></v-star>
            <span class="score">{{seller.foodScore}}</span>
          </div>
          <div class="deliveryTime-wrapper">
            <span class="title">送达时间</span>
            <span class="text">{{seller.deliveryTime}}分钟</span>
          </div>
        </div>
      </div>
      <v-split></v-split>
      <v-ratingselect :ratings="ratings" :only-content="onlyContent" :select-type="selectType"></v-ratingselect>
      <div class="rating-wrapper">
         <ul>
          <li v-for="rating in ratings" class="rating-item" v-bind:key="rating.username">
            <div class="avatar">
              <img :src="rating.avatar" alt="" width="28" height="28">
            </div>
            <div class="content">
              <h1 class="name">{{rating.username}}</h1>
              <div class="star-wrapper">
                <v-star :size="24" :score="rating.score"></v-star>
                <span class="delivery" v-show="rating.deliveryTime">{{rating.deliveryTime}}</span>
              </div>
              <p class="text">{{rating.text}}</p>
              <div class="recommend" v-show="rating.recommend && rating.recommend.length > 0">
                <span class="icon-thumb_up"></span>
                <span v-for="item in rating.recommend" v-bind:key="item">{{item}}</span>
              </div>
              <div class="time">{{rating.rateTime | formatDate}}</div>
            </div>
          </li>
        </ul>
      </div>
     
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  import BScroll from 'better-scroll';
  import star from '../star/star.vue';
  import split from '../split/split.vue';
  import ratingselect from '../ratingselect/ratingselect.vue';
  import {formatDate} from '../../common/js/date';

  const ALL = 2;
  const ERR_OK = 200;
  export default {
    data () {
          return {
            ratings: [],
            selectType: ALL,
            onlyContent: true,
            betterScorll: null
          };
    },
    props: {
      seller: {
        type: Object
      }
    },
    components: {
        'v-star': star,
        'v-split': split,
        'v-ratingselect': ratingselect
    },
    created () {
      this.$http.get('/api/ratings').then((res) => {
        let body = res.body;
        if (body.errno === ERR_OK) {
          this.ratings = body.data;
          this.$nextTick(() => {
            this.betterScorll = new BScroll(this.$els.ratingsw, {
               click: true
            });
          });
        }
      });
    },
    filters: {
        formatDate (time) {
            let date = new Date(time);
            return formatDate(date, 'yyyy-MM-dd hh:mm');
        }
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import '../../common/stylus/minin.styl';
  .ratings
    position absolute
    top 174px
    left 0
    bottom 0
    width 100%
    overflow hidden
    .overview
      display flex
      padding 18px 0
      .overview-left
        flex 0 0 137px
        padding 6px 0
        width 137px
        border-right 1px solid rgba(7,17,27,0.1)
        text-align center
        @media only screen and (max-width 320px)
          flex 0 0 120px
          width 120px
        .score
          line-height 28px
          font-size 24px
          color rgb(255,153,0)
          margin-bottom 6px
        .title
          line-height 12px
          font-size 12px
          color rgb(7,17,27)
          margin-bottom 8px
        .rank
          line-height 10px
          font-size 10px
          color rgb(147,153,159)
      .overview-right
        flex 1
        padding 6px 0 6px 24px
        @media only screen and (max-width 320px)
          padding-left 6px
        .score-wrapper
          margin-bottom 8px
          font-size 0
          .title
            line-height 18px
            display inline-block
            vertical-align top 
            font-size 12px
            color rgb(7,17,27)
          .star
            display inline-block
            vertical-align top 
            margin 0 12px
          .score
            display inline-block
            vertical-align top
            font-size 12px
            color rgb(255,153,0)
            line-height 18px
        .deliveryTime-wrapper
          font-size 0
          .title
            line-height 18px
            font-size 12px
            color rgb(7,17,27)
          .text
            margin 0 12px
            line-height 18px
            color rgb(147,153,159)
            font-size 12px
    .rating-wrapper
      padding 0 18px
      .rating-item
        display flex
        padding 18px 0
        border-1px(rgba(7,17,27,0.1))
</style>
