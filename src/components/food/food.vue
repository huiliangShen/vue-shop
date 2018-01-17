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
            <v-split></v-split>
            <div class="info" v-show="food.info">
                <h1 class="title">商品信息</h1>
                <p class="text">{{food.info}}</p>
            </div>
             <v-split></v-split>
             <div class="rating">
                 <div class="title">商品评价</div>
                 
             </div>
        </div>
       
    </div>
</template>

<script type="text/ecmascript-6">
  import BetterScroll from 'better-scroll';
  import cartcontrol from '../cartcontrol/cartcontrol.vue';
  import Vue from 'vue';
  import split from '../split/split.vue';
  export default {
      props: {
          food: {
              type: Object
          }
      },
      data () {
          return {
              showFlag: false,
              scroll: null
          };
      },
      components: {
          'v-cartcontrol': cartcontrol,
          'v-split': split
      },
      methods: {
          show () {
              this.showFlag = true;
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
          }
      }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
    .food
        position fixed
        top 0
        left 0
        bottom 46px
        z-index 30
        width 100%
        height 100%
        background #fff
        &.move-transition
            transition all 0.2s linear
            transform translate3d(0,0,0)
        &.move-enter, &.move-leave
            transform translate3d(100%,0,0)
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
</style>    
