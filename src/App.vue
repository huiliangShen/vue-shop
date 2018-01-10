<template>
  <div>
    <v-header :seller="seller"></v-header>
    <div class="tab border-1px">
      <div class="tab-item"> <a v-link="{ path: '/goods' }">商品</a></div>
      <div class="tab-item"> <a v-link="{ path: '/ratings' }">评论</a></div>
      <div class="tab-item"> <a v-link="{ path: '/seller' }">商家</a></div>
    </div>
    <!-- 路由外链 -->
    <router-view :seller="seller"></router-view>
  </div>
</template>

<script>
  import header from './components/hedaer/header.vue';

  const ERR_OK = 200;

  export default {
      data () {
          return {
              seller: {}
          };
      },
      components: {
          'v-header': header
      },
      created() {
          let vm = this;
          this.$http.get('/api/seller').then((res) => {
              let body = res.body;
              if (body.errno === ERR_OK) {
                vm.seller = body.data;
              }
          });
      }
  };
</script>

<style lang="stylus" rel="stylesheet/stylus">
  @import './common/stylus/minin.styl';
  .tab{
    display: flex;
    width: 100%;
    height: 40px;
    line-height: 40px;
    border-1px(rgba(7,17,27,0.1));
    /*border-bottom 1px solid rgba(7,17,27,0.1);*/
  }
  .tab > .tab-item{
    flex: 1;
    text-align: center;
  }

  .tab > .tab-item a{
    text-decoration: none;
    display: block;
    font-size: 14px;
    /*line-height: 14px;*/
    color: rgb(77,85,93);
  }

  .tab > .tab-item a.active {
    color: rgb(240,20,20);
  }
</style>
