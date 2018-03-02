<template>
    <div class="food" v-show="showFlag" transition="move" v-el:food>
        <div class="food-content">
            <div class="img-header">
                <img :src="food.image" alt="">
                <div class="back" @click="back()">
                    <i class="icon-arrow_lift"></i>
                </div>
            </div>
            <div class="content">
                <h1 class="title">{{food.name}}</h1>
                <div class="detail">
                    <span class="sell-count">月售{{food.sellCount}}份</span><span class="rating">好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                  <span>￥{{food.price}}</span><span v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                </div>
                 <div class="cartcontrol-wrapper">
                    <v-cartcontrol :food="food"></v-cartcontrol>
                </div>
                <div class="buy" v-show="!food.count || food.count === 0" @click="addFood($event)" transition="fade">加入购物车</div>
            </div>
            <v-split v-show="food.info"></v-split>
            <div class="info" v-show="food.info">
                <h1 class="title">商品信息</h1>
                <p class="text">{{food.info}}</p>
            </div>
             <v-split></v-split>
             <div class="rating">
                <h1 class="title">商品评价</h1>
                <v-ratingselect :ratings="food.ratings" :only-content="onlyContent" :desc="desc" :select-type="selectType"></v-ratingselect>
                <div class="rating-wrapper">
                    <ul v-if="food.ratings && food.ratings.length">
                        <li v-for="(rating, $index) in food.ratings" class="rating-item border-1px" v-show="needShow(rating.rateType, rating.text)" v-bind:key="$index">
                            <div class="user">
                                <span class="name">{{rating.username}}</span>
                                <img class="avatar" :src="rating.avatar" alt="" width="12" height="12">
                            </div>
                            <div class="time">{{rating.rateTime | formatDate}}</div>
                            <p class="text">
                                <span class="icon" :class="{'icon-thumb_up': rating.rateType === 0, 'icon-thumb_down': rating.rateType === 1}"></span><span>{{rating.text}}</span>
                            </p>
                        </li>
                    </ul>
                    <div class="no-rating" v-else>
                        暂无数据
                    </div>
                </div>
             </div>
        </div>
       
    </div>
</template>

<script type="text/ecmascript-6">
  import BetterScroll from 'better-scroll';
  import cartcontrol from '../cartcontrol/cartcontrol.vue';
  import Vue from 'vue';
  import split from '../split/split.vue';
  import ratingselect from '../ratingselect/ratingselect.vue';
  import {formatDate} from '../../common/js/date';

// const POSITIVE = 0;
// const NEGATIVE = 1;
const ALL = 2;
  export default {
      props: {
          food: {
              type: Object
          }
      },
      data () {
          return {
              showFlag: false,
              scroll: null,
              selectType: ALL,
              onlyContent: true,
              desc: {
                  all: '全部',
                  positive: '推荐',
                  negative: '吐槽'
              }
          };
      },
      components: {
          'v-cartcontrol': cartcontrol,
          'v-split': split,
          'v-ratingselect': ratingselect
      },
      methods: {
          show () {
              this.showFlag = true;
            // this.selectType = ALL;
            // this.onlyContent = true;
              this.$nextTick(() => {
                  if (!this.scroll) {
                      this.scroll = new BetterScroll(this.$els.food, {
                          click: true
                      });
                  } else {
                      this.scroll.refresh();
                  }
              });
          },
          back () {
              this.showFlag = false;
          },
          addFood (event) {
              if (!event._constructed) {
                  return;
              }
              Vue.set(this.food, 'count', 1);
              this.$dispatch('cart.add', event.target);
          },
          needShow (type, text) {
              if (this.onlyContent && !text) {
                  return false;
              }
              if (this.selectType === ALL) {
                  return true;
              } else {
                  return type === this.selectType;
              }
          }
      },
      filters: {
          formatDate (time) {
              let date = new Date(time);
              return formatDate(date, 'yyyy-MM-dd hh:mm');
          }
      },
      events: {
          ratingtype (number) {
              this.selectType = number;
              this.$nextTick(() => {
                this.scroll.refresh();
              });
          },
           chooseContent (onlyContent) {
              this.onlyContent = onlyContent;
               this.$nextTick(() => {
                this.scroll.refresh();
              });
          }
      }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
@import "../../common/stylus/minin.styl";
    .food
        position fixed
        top 0
        left 0
        bottom 48px
        z-index 30
        width 100%
        //height 100%
        background #fff
        &.move-transition
            transition all 0.2s linear
            transform translate3d(0,0,0)
        &.move-enter, &.move-leave
            transform translate3d(0,100%,0)
        .img-header
            position relative
            width 100%
            height 0
            padding-top 100%
            img 
                position absolute
                top 0
                left 0
                width 100%
                height 100%
            .back
                position absolute
                top 10px
                left 5px
                background rgba(7,17,27,0.2)
                border-radius 50%
                text-align center
                .icon-arrow_lift
                    display block
                    padding 12px
                    color #ffffff
                    font-size 15px
        .content
            position relative
            padding 18px
            .title
                font-size 14px
                font-weight 700
                color rgb(7,17,27)
                margin-bottom 8px
                line-height 14px
            .detail
                margin-bottom 18px
                height 10px
                line-height 10px
                font-size 0
                .sell-count, .rating
                    font-size 10px
                    color rgb(147,153,159)
                .sell-count
                    margin-right 12px
            .price
              font-size 0
              font-weight 700
              line-height 24px
              & > span:first-child
                font-size 14px
                color red
                margin-right 8px
              & > span:first-child + span                 
                font-size 10px
                color rgb(147,152,159)
                text-decoration line-through
            .cartcontrol-wrapper
                position absolute
                right 12px
                bottom 12px
            .buy
                position absolute
                right 18px
                bottom 18px
                z-index 10
                height 24px
                line-height 24px
                padding 0 12px
                box-sizing border-box
                font-size 10px
                border-radius 12px
                color #ffffff
                background rgb(0,160,220)
                &.fade-transition
                    transition all 0.5s linear
                    opacity 1
                &.fade-enter,&.fade-leave
                    opacity 0
        .info
            padding 18px
            .title
                line-height 14px
                margin-bottom 6px
                font-size 14px
                color rgb(7,17,27)
            .text
                line-height 24px
                font-size 12px
                padding 0 8px
                color rgb(77, 85,93)
        .rating
            padding-top 18px
            .title
                line-height 14px
                margin-left 18px
                font-size 14px
                color rgb(7,17,27)
            .rating-wrapper
                padding 0 18px
                .rating-item
                    position relative
                    padding 16px 0
                    border-1px(rgba(7,17,27,0.1))
                    .user
                        position absolute
                        right 0
                        top 16px
                        line-height 12px
                        font-size 0
                        .name
                            display inline-block
                            vertical-align top
                            color rgb(147,153,159)
                            font-size 10px
                            margin-right 6px
                        .avatar
                            border-radius 50%
                    .time
                        margin-bottom 6px
                        font-size 10px
                        color rgb(147,153,159)
                        line-height 12px
                    .text
                        font-size 12px
                        .icon
                            line-height 24px
                            margin-right 4px
                            font-size 12px
                            &.icon-thumb_up
                                color rgb(0,160,220)
                            &.icon-thumb_down
                                color rgb(147,153,159)
                        &:last-of-type
                            color rgb(7,17,27)
                            line-height 16px
                .no-rating
                    padding 16px 0
                    font-size 12px
                    color rgb(147,153,159)
                    line-height 12px
</style>    
