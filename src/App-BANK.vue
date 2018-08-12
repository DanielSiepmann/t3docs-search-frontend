<template>
  <div id='defaultForm'>
  </div>
</template>

<script>
  /*
      Simple way to use InnerSearch
  */

  import Vue from 'vue';
  import {Searchbox,
    SearchDatalist,
    RefinementListFilter,
    Paginate,
    SearchButton,
    ResetButton,
    Hits,Generics,NumericListFilter} from 'vue-innersearch/src/innerSearch';



      Vue.component('searchbox', Searchbox);
      Vue.component('search-datalist', SearchDatalist);
      Vue.component('refinement-list-filter', RefinementListFilter);
      Vue.component('paginate', Paginate);
      Vue.component('search-button', SearchButton);
      Vue.component('reset-button', ResetButton);
      Vue.component('hits', Hits);
      Vue.component('numeric-list-filter', NumericListFilter);

      Vue.mixin(Generics);

  window.addEventListener('load', function () {
    new Vue({
      el: '#defaultForm',

      created : function () {
        // ES server configuration
        this.setHost('http://t3-elasticsearch.at.localhost/');
        this.setIndex('typo3documentation');
        this.setType('manualentry');
      },

      template : `
                <section>
                    <h1 class='is-title'>innerSearch.js</h1>

                    <hr class='is-line' />

                    <div class="is-columns">
                        <div class="is-column is-one-fifth">
                            <div>
                                <refinement-list-filter :field="'manual'" :search="true" :size="100" :title="'Manual : '" :displayCount="true" operator="AND" ></refinement-list-filter>
                                <refinement-list-filter :field="'version'" :search="true" :size="100" :title="'Version : '" :displayCount="true" operator="AND" ></refinement-list-filter>
                            </div>
                        </div>
                        <div class="is-column">
                            <div>
                                <searchbox :autofocus="true" :realtime="true" :timeout="200" :field="'content'" :placeholder="'Search content'"></searchbox>

                                <div style="margin: 20px auto;width: 90%">
                                    <search-button></search-button>
                                    <reset-button></reset-button>
                                </div>
                                <hits>
                                    <template slot="hits" slot-scope="{ hits }">
                                        <div class="is-score is-hits">
                                            <strong v-if="hits.score === 0">No result found</strong>
                                            <strong v-else-if="hits.score === 1">1 result found</strong>
                                            <strong v-else-if="hits.score > 1">{{ hits.score }} results found</strong>
                                        </div>
                                        <div v-for="item in hits.items" :item="item">
                                            <div>
                                                <a :href="item._source.uri"><strong>{{ item._source.title }}</strong></a><br>
                                                <small>Manual: {{ item._source.manuel }} Version: {{ item._source.version }}</small><br>
                                                {{ item._source.content }}
                                            </div>
                                        </div>
                                    </template>
                                </hits>

                                <paginate :previousText="'&#x2B9C; Previous page'" :nextText="'Next page &#x2B9E;'" :size="10"></paginate>
                            </div>
                         </div>
                    </div>
                </section>
            `
    });
  });

  export default {};
</script>

<style>
  @import url(https://unpkg.com/vue-innersearch@0.0.10/default-innersearch-theme.min.css)
</style>
<style>
    .is-field.is-searchbox { background-color: #fff; }
</style>
