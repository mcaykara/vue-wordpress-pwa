<template>
  <section id="ThemeCategory">
    <div class="columns personal-card card"  v-if="routeParamId == 'blog'">
      <p class="column personal-img"><img src="https://api.fullstackweekly.com/wp-content/uploads/2016/11/200x200.jpg" alt="Bill Stavroulakis" width="100"></p>
      <p class="column is-three-quarters personal-desc">Hi, I’m <a href="https://twitter.com/bstavroulakis" rel="noopener" target="_blank">Bill Stavroulakis</a>, many years ago my journey started on this thing called Web Development.<br/><br/>
      Over here you can find all of the interesting things I find on my way.
      </p>
    </div>
    <div class="columns"><div class="column">
      <div class="vwp-loading" v-if="loading">
          <img alt="loading" src="../assets/loading.gif" />
      </div>
    </div></div>
    <div class="columns">
      <div class="column" v-if="category.name">
        <h2>
            {{category.name}}
        </h2>
        <vwp-subcategory v-if="category" :category="category"></vwp-subcategory>
      </div>
    </div>
  </section>
</template>

<script>
import { mapGetters } from 'vuex'
let wordpressService;
export default {
  name: 'ThemeCategory',
    components: { 
    'vwp-subcategory': require('../components/vwpSubcategory.vue'), 
    'app-newsletter': require('./shared/AppNewsletter.vue') 
  },
  data: () => {
    return { 
      category: {},
      loading: true
    }
  },
  computed: {
    ...mapGetters([
      'routeParamId',
      'routeParams'
    ])
  },
  watch: {
    routeParamId : function(newCategorySlug){
      this.updateCategory(newCategorySlug);
    }
  },
  methods: {
    updateCategory: function(categorySlug){
      var self = this;
      require.ensure('../app.service.js', function(){
        wordpressService = require('../app.service.js').default;
        wordpressService.getCategory(self, null, categorySlug).then((category) => {
          if(category && category.length > 0){
            self.category = category[0];
            self.loading = false;
          }
        })
      });
    }
  },
  created () {
    var categorySlug = this.routeParamId;
      if(this.routeParams.category){
        categorySlug = this.routeParams.category
      }
    this.updateCategory(categorySlug);
  }
}
</script>

<style lang="scss">
  #ThemeCategory{
    .personal-card{
      background-color: #ffffff;
      margin-right: 0;
      margin-left: 0;
      .personal-img{
        text-align: center;
        align-self: center;
        margin-bottom:0;
      }
      .personal-desc{
        align-self: center;
      }
    }
    .category-posts{
      flex-wrap:wrap;
    }
   .hero-body{
      padding-top: 0;
    }
  }
</style>
