<template>
  <div class="shopcart">
      <div class="content">
          <div class="content-left">
              <div class="logo-wrapper">
                <div class="logo" :class="{'highlight': totalCount > 0}">
                    <i class="icon-shopping_cart" :class="{'highlight': totalCount > 0}"></i>
                </div>
                <div class="num" v-show="totalCount > 0">{{totalCount}}</div>
              </div>
              <div class="price"  :class="{'highlight': totalPrice > 0}">￥{{totalPrice}}</div>
              <div class="desc">另需配送费￥{{deliveryPrice}}元</div>
          </div>
          <div class="content-right">
              <div class="pay" :class="{'highlight': totalPrice >= minPrice}">
                 {{payDesc}}
              </div>
          </div>
      </div>
      <div class="ball-container">
          <div transition="drop" v-for="ball in balls" v-show="ball.show" class="ball">
              <div class="inner inner-hook"></div>
          </div>
      </div>
  </div>
</template>

<script>
export default {
    data () {
        return {
            balls: [
                {
                    show: false
                },
                {
                    show: false
                },
                {
                    show: false
                },
                {
                    show: false
                },
                {
                    show: false
                }
            ],
            dropBalls: []
        };
    },
    props: {
        selectFoods: {
            type: Array,
            default () {
                return [
                    {
                        price: 20,
                        count: 0
                    }
                ];
            }
        },
        deliveryPrice: {
            type: Number
        },
        minPrice: {
            type: Number
        }
    },
    computed: {
        totalCount () {
            let num = 0;
            for (let item of this.selectFoods) {
                num += item.count;
            }
            return num;
        },
        totalPrice () {
            let price = 0;
            for (let item of this.selectFoods) {
                price += item.count * item.price;
            }
            return price;
        },
        payDesc () {
            if (this.totalPrice === 0) {
                return `￥${this.minPrice}起送`;
            } else if (this.totalPrice < this.minPrice) {
                return `还差￥${this.minPrice - this.totalPrice}起送`;
            } else {
                return '去结算';
            }
        }
    },
    methods: {
        drop (el) {
             for (let i = 0; i < this.balls.length; i++) {
                let ball = this.balls[i];
                if (!ball.show) {
                    ball.show = true;
                    ball.el = el;
                    this.dropBalls.push(ball);
                    return;
                }
            }
        }
    },
    transitions: {
        drop: {
            beforeEnter: function (el) {
                let count = this.balls.length;
                while (count--) {
                    let ball = this.balls[count];
                    if (ball.show) {
                        // 获取当前元素相对于视口的位置
                        let rect = ball.el.getBoundingClientRect();
                        let x = rect.left - 32;
                        let y = -(window.innerHeight - rect.top - 22);
                        el.style.display = 'block';
                        el.style.webkitTransform = `translate3d(0,${y}px,0)`;
                        el.style.transform = `translate3d(0,${y}px,0)`;
                        let inner = el.getElementsByClassName('inner-hook')[0];
                        inner.style.webkitTransform = `translate3d(${x}px,0,0)`;
                        inner.style.transform = `translate3d(${x}px,0,0)`;
                    }
                }
            },
            enter: function (el) {
                /* eslint-disable no-unused-vars */
                let rf = el.offsetHeight;
                this.$nextTick(() => {
                    el.style.webkitTransform = 'translate3d(0,0,0)';
                    el.style.transform = 'translate3d(0,0,0)';
                    let inner = el.getElementsByClassName('inner-hook')[0];
                    inner.style.webkitTransform = 'translate3d(0,0,0)';
                    inner.style.transform = 'translate3d(0,0,0)';
                });
            },
            afterEnter: function (el) {
                let ball = this.dropBalls.shift();
                if (ball.show) {
                    ball.show = false;
                    ball.el = '';
                    el.style.display = 'none';
                }
            }
        }
    }
};
</script>

<style lang="stylus" rel="stylesheet/stylus">
    .shopcart
        position fixed
        left 0
        bottom 0
        z-index 50
        width 100%
        height 48px
        .content
            display flex
            background #141d27
            font-size 0
            .content-left
                flex 1
                .logo-wrapper
                    display inline-block
                    position relative
                    top -10px
                    margin 0 12px
                    padding 6px
                    width 56px
                    height 56px
                    box-sizing border-box
                    vertical-align top
                    border-radius 50%
                    background #141d27
                    .logo
                        width 100%
                        height 100%
                        border-radius 50%
                        background #2b343c
                        text-align center
                        &.highlight
                            background rgb(0,160,220)
                        .icon-shopping_cart
                            font-size 24px
                            color: #80858a
                            line-height 44px
                            &.highlight
                                color #ffffff
                .num
                    position absolute
                    top 0
                    right 0
                    width 24px
                    height 16px
                    line-height 16px
                    text-align center
                    border-radius 16px
                    font-size 9px
                    font-weight 700
                    color #ffffff
                    background rgb(240,20,20)
                    box-shadow 0 4px 8px 0 rgba(0,0,0,0.4)
                .price
                    display inline-block
                    vertical-align top
                    margin-top 12px
                    line-height 24px
                    box-sizing border-box
                    border-right 1px solid rgba(255,255,255,0.1)
                    font-size 16px
                    font-weight 700
                    line-height 24px
                    color rgba(255,255,255,0.4)
                    padding-right 12px
                    &.highlight
                        color rgb(255,255,255)
                .desc
                    display inline-block
                    vertical-align top
                    line-height 24px
                    margin 12px 0 0 12px
                    font-size 10px
                    color rgba(255,255,255,0.4)
            .content-right
                flex 0 0 105px
                width 105px
                .pay
                    font-size 12px
                    height 48px
                    line-height 48px
                    font-weight 700
                    color rgba(255,255,255,0.4)
                    text-align center
                    background #2b333b
                    &.highlight
                        color #ffffff
                        background #00b43c
        .ball-container
            .ball
                position fixed
                left 32px
                bottom 35px
                z-index 200
                &.drop-transition
                    transition all 0.4s cubic-bezier(.49,-0.29,.75,.21)
                    .inner
                        width 16px
                        height 16px
                        border-radius 50%
                        background rgb(0,160,220)
                        transition all 0.4s linear

</style>

