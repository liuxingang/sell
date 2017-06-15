<template>
  <div>
    <v-header :seller="seller"></v-header>
    <div class="tab">
      <div class="tab-item">
        <a v-link="{path:'/goods'}">商品</a>
      </div>
      <div class="tab-item">
        <a v-link="{path:'/ratings'}">评论</a>
      </div>
      <div class="tab-item">
        <a v-link="{path:'/seller'}">商家</a>
      </div>
    </div>
    <router-view :seller="seller" keep-alive></router-view>
  </div>
</template>

<script>
  import {urlParse} from 'common/js/utils';
  import Header from 'components/header/header.vue';

  const ERR_OK = 0;

  export default {
    data () {
      return {
        seller: {
          id: (() => {
            let queryParam = urlParse();
            console.log(queryParam);
            return queryParam.id;
          })()
        }
      };
    },
    created () {
      this.$http.get('/api/seller').then((response) => {
        response = response.body;
        if (response.errno === ERR_OK) {
          this.seller = Object.assign({}, this.seller, response.data);
        }
      });
    },
    components: {
      'v-header': Header
    }
  };
</script>

<style>
  .tab {
    display: flex;
    width: 100%;
    height: 40px;
    line-height: 40px;
    border-bottom: 1px solid rgba(7, 17, 27, 0.1);
  }

  .tab .tab-item {
    text-align: center;
    flex: 1;
  }

  .tab .tab-item a {
    display: block;
    font-size: 14px;
    color: rgb(77, 85, 93);
  }

  .tab .tab-item a.active {
    color: #f01414;
  }
</style>
