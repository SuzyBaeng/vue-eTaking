<template>
  <div id="app">
    <v-header :seller="seller"></v-header>
    <div class="tab border-1px">
    	<div class="tab-item">
    		<router-link to="/goods" class="mett">商品</router-link>
    	</div>
    	<div class="tab-item">
    		<router-link to="/ratings" class="mett">评论</router-link>
    	</div>
    	<div class="tab-item">
    		<router-link to="/seller" class="mett">商家</router-link>
    	</div>
    </div>
    <keep-alive>
    <router-view :seller="seller"></router-view>
    </keep-alive>
  </div>
</template>

<script type="text/ecmascript-6">
	import {urlParse} from 'common/js/util';
  import header from './components/header/header.vue';

  const ERR_OK = 0;
//const debug = process.env.NODE_ENV !== 'production';

  export default {
    data() {
      return {
        seller: {
          id: (() => {
            let queryParam = urlParse();
            return queryParam.id;
          })()
        }
      };
    },
    created() {
      const url = '/api/seller';
      this.$http.get(url + '?id=' + this.seller.id).then((response) => {
        response = response.body;
        if (response.errno === ERR_OK) {
          this.seller = Object.assign({}, this.seller, response.data);
        }
      });
    },
    components: {
      'v-header': header
    }
  };
</script>
<style lang="stylus" type="text/sheetstylus">
 @import "./common/stylus/mixin.styl"

	#app
		.tab
			display: flex
			width: 100%
			height: 40px
			line-height: 40px
			border-1px(rgba(7, 17, 27, 0.1))
			.tab-item
				flex: 1
				text-align: center
				a.mett
				 	display: block
				 	font-size: 14px
				 	color: rgb(77, 85, 93)
				a.active
				 	color: rgb(240, 20, 20)
				 
</style>
