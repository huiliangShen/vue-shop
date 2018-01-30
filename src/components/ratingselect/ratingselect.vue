<template>
    <div class="ratingselect">
        <div class="rating-type border-1px">
            <span class="block positive" :class="{'active': selectType === 2}" @click="chooseType(2,$event)">{{desc.all}}<span class="count">{{ratings.length}}</span></span>
            <span class="block positive" :class="{'active': selectType === 0}" @click="chooseType(0,$event)">{{desc.positive}}<span class="count">{{positives}}</span></span>
            <span class="block negative" :class="{'active': selectType === 1}" @click="chooseType(1,$event)">{{desc.negative}}<span class="count">{{negatives}}</span></span>
        </div>
        <div class="switch" :class="{'on': onlyContent}" @click="chooseContent($event)">
            <span class="icon-check_circle"></span><span class="text">只看有内容的评价</span>
        </div>
    </div>
</template>

<script type="text/ecmascript-6">
const POSITIVE = 0;
const NEGATIVE = 1;
const ALL = 2;
  export default {
      props: {
          ratings: {
              type: Array,
              default () {
                  return [];
              }
          },
          selectType: {
              type: Number,
              default: ALL
          },
          onlyContent: {
              type: Boolean,
              default: false
          },
          desc: {
              type: Object,
              default () {
                  return {
                      all: '全部',
                      positive: '满意',
                      negative: '不满意'
                  };
              }
          }
      },
      computed: {
          positives () {
              return this.ratings.filter((e) => {
                  return e.rateType === POSITIVE;
              }).length;
          },
          negatives () {
              return this.ratings.filter((e) => {
                  return e.rateType === NEGATIVE;
              }).length;
          }
      },
      methods: {
          chooseType (number, event) {
             if (!event._constructed) {
                 return;
              }
              this.selectType = number - 0;
              this.$dispatch('ratingtype', number);
          },
          chooseContent (event) {
              if (!event._constructed) {
                  return;
              }
              this.onlyContent = !this.onlyContent;
              this.$dispatch('chooseContent', this.onlyContent);
          }
      }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
    @import "../../common/stylus/minin.styl";
    .ratingselect
        .rating-type
            padding 18px 0
            margin 0 18px
            border-1px(rgba(7,17,27,0.1))
            font-size 0
            .block
                display inline-block
                padding 8px 12px
                margin-right 8px
                border-radius 1px
                color: rgb(77,85,93)
                font-size 12px
                line-height 16px
                &.active
                    color #ffffff
                .count
                    margin-left 2px
                    font-size 8px
                    vertical-align top
                &.positive
                    background rgba(0,160,220,0.2)
                    &.active
                        background rgb(0,160,220)
                &.negative
                    background rgba(77,85,93,0.2)
                    &.active
                        background rgb(77,85,93)
        .switch
            padding 12px 18px
            line-height 24px
            border-bottom 1px solid rgba(7,17,27,0.1)
            color rgb(147,153,159)
            font-size 0
            &.on
                .icon-check_circle
                    color #00c850
            .icon-check_circle
                display inline-block
                vertical-align top
                font-size 24px
                margin-right 4px
            .text
                display inline-block
                vertical-align top
                font-size 12px
        


</style>
