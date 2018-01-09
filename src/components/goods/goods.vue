<template>
  <div class="goods">
    <div class="menu-wrapper">
      <ul>
        <li v-for="item in goods" track-by="$index" class="menu-item">
          <span class="text border-1px">
            <span  v-show="item.type > 0" class="icon" :class="classMap[item.type]"></span>{{item.name}}
            </span>
        </li>
      </ul>
    </div>
    <div class="foods-wrapper">
      <ul>
        <li v-for="item in goods" class="food-list">
          <h1 class="title">{{item.name}}</h1>
          <ul>
            <li v-for="food in item.foods" class="food-item border-1px">
              <div class="icon">
                <img :src="food.icon" alt="" width="57" height="57">
              </div>
              <div class="content">
                <h1 class="name">{{food.name}}</h1>
                <p class="des">{{food.description}}</p>
                <div class="extra">
                  <span>月售{{food.sellCount}}份</span><span>好评率{{food.rating}}%</span>
                </div>
                <div class="price">
                  <span>￥{{food.price}}</span>
                  <span v-show="food.oldPrice">￥{{food.oldPrice}}</span>
                </div>
              </div>
            </li>
          </ul>
        </li>
      </ul>
    </div>
  </div>
</template>

<script type="text/ecmascript-6">
  const ERR_OK = 200;
  export default {
    data () {
      return {
        goods: [],
        classMap: []
      };
    },
    props: {
      seller: {
        type: Object
      }
    },
    created () {
      let vm = this;
      this.$http.get('/api/goods').then((res) => {
        let result = res.body;
        if (result.errno === ERR_OK) {
            vm.goods = result.data;
        }
      });
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
    }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
    @import "../../common/stylus/minin.styl";
    .goods
      display flex
      position absolute
      top 174px
      bottom 46px
      width 100%
      overflow hidden
      .menu-wrapper
        flex 0 0 80px
        //不设置 安卓有问题
        width 80px
        background #f3f5f7
        .menu-item
          //垂直居中最好方法
          display table
          padding 0 12px
          height 54px
          width 56px
          line-height 14px
          .icon
            display inline-block
            vertical-align top
            width 12px
            height 12px
            margin-right 2px
            background-size 12px 12px
            background-repeat no-repeat
            &.decrease
              bg-image('decrease_3')
            &.discount
              bg-image('discount_3')
            &.guarantee
              bg-image('guarantee_3')
            &.invoice
              bg-image('invoice_3')
            &.special
              bg-image('special_3')
          .text
            display table-cell
            width 56px
            vertical-align middle
            border-1px(rgba(7,17,27,0.1))
            font-size 12px
      .foods-wrapper
        flex 1
        .title
          padding-left 14px
          height 26px
          line-height 26px
          font-size 12px
          color rgb(147,153,159)
          border-left 2px solid #d9dde1
          background #f3f5f7
        .food-item
          display flex
          margin 18px
          padding-bottom 18px
          border-1px(rgba(7,17,27,0.1))
          &:last-child
            border-none()
            margin-bottom 0
          .icon
            flex 0 0 57px
            margin-right 10px
          .content
            flex 1
            .name
              margin 2px 0 8px 0
              height 14px
              line-height 14px
              font-size 14px
              color rgb(7,17,27)
            .des
              font-size 10px
              color rgb(147,152,159)
              margin-bottom 8px
              line-height 10px
            .extra
              line-height 10px
              font-size 10px
              color rgb(147,152,159)
              margin-bottom 8px
              & > span:first-child
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
</style>
