<template>
  <div class="cartcontrol">
      <div class="cart-descrease" v-show="food.count > 0" @click.prevent.stop="deleteItem($event)" transition="move">
          <span class="inner icon-remove_circle_outline"></span>
      </div>
      <div class="cart-count" v-show="food.count > 0">{{food.count}}</div>
      <div class="cart-add icon-add_circle" @click.prevent.stop="add($event)">
      </div>
  </div>
</template>
<script>
import Vue from 'vue';
export default {
    props: {
        food: {
            type: Object
        }
    },
    methods: {
        add (event) {
            if (!event._constructed) {
                return;
            }
            if (!this.food.count) {
                // this.food.count = 1;
                Vue.set(this.food, 'count', 1);
            } else {
                this.food.count++;
            }
            this.$dispatch('cart.add', event.target);
        },
        deleteItem(event) {
             if (!event._constructed) {
                return;
            }
            if (this.food.count) {
                // this.food.count = 1;
               this.food.count--;
            }
        }
    }
};
</script>

<style lang="stylus">
    .cartcontrol
        font-size 0
        .cart-descrease
            display inline-block
            padding 6px 
        .cart-descrease
            transition all 0.4s linear 
            &.move-transition
                opacity 1
                transform translate3d(0, 0, 0)
                .inner
                    display inline-block
                    font-size 24px
                    line-height 24px
                    color rgb(0,160,220)
                    transition  all 0.4s linear
                    transform rotate(0)
            &.move-enter, &.move-leave
                opacity 0
                transform translate3d(24px, 0, 0)
                .inner
                    transform rotate(180deg)
        .cart-count
            display inline-block
            vertical-align top
            width 12px
            padding-top 6px
            font-size 10px
            line-height 24px
            color rgb(147,153,159)
            text-align center
        .cart-add
            display inline-block
            padding 6px 
            font-size 24px
            line-height 24px
            color rgb(0,160,220)
</style>

