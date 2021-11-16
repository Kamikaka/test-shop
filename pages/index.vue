<template>
  <div class="catalog-list page-mt row">
    <div class="page-left-block">
      <div class="catalog_filters" id="filter" v-bind:class="{hide_filter: !showFilter}">
        <div class="title_block row ai_center">
          <div class="caption">Фильтр</div>
          <div class="mobile_filter_open bg-box row ai_center jc_center" v-on:click="showFilter = !showFilter">
            <svg-icon name="chevron-down" />
          </div>
        </div>
        <div class="catalog_filter bg-box">
          <div class="catalog_filter_items">
            <div class="catalog_filter_title">Multi Range</div>
            <div class="catalog_filter_item">
              <label class="input-radio">
                <input type="radio" value="10" name="price" @change="catalog_price.valMin = catalog_price.min, catalog_price.valMax = 10, catalog_pageOfItems = catalog_items_filter">
                <div class="caption">$10</div>
              </label>
              <label class="input-radio">
                <input type="radio" value="100" name="price" @change="catalog_price.valMin = 11, catalog_price.valMax = 100, catalog_pageOfItems = catalog_items_filter">
                <div class="caption">$10 - $100</div>
              </label>
              <label class="input-radio">
                <input type="radio" value="500" name="price" @change="catalog_price.valMin = 101, catalog_price.valMax = 500, catalog_pageOfItems = catalog_items_filter">
                <div class="caption">$100 - $500</div>
              </label>
              <label class="input-radio">
                <input type="radio" value="500" name="price" @change="catalog_price.valMin = 500, catalog_price.valMax = catalog_price.max, catalog_pageOfItems = catalog_items_filter">
                <div class="caption">$500</div>
              </label>
              <label class="input-radio">
                <input type="radio" value="" checked name="price" @change="catalog_price.valMin = catalog_price.min, catalog_price.valMax = catalog_price.max, catalog_pageOfItems = catalog_items_filter">
                <div class="caption">All</div>
              </label>
            </div>
          </div>
          <div class="catalog_filter_items">
            <div class="catalog_filter_title">Slider</div>
            <div class="catalog_filter_item">
            </div>
          </div>
          <div class="catalog_filter_items">
            <div class="catalog_filter_title">Category</div>
            <div class="catalog_filter_item">
              <label class="input-checkbox" v-for="category in catalog_category" :key="category.id">
                <input type="checkbox" :value="category.id" name="category" @change="filterCatalog($event)" v-model="category.active">
                <div class="caption">{{category.name}}</div>
              </label>
            </div>
          </div>
          <div class="catalog_filter_items">
            <div class="catalog_filter_title">Brand</div>
            <div class="catalog_filter_item">
              <label class="input-checkbox" v-for="brand in catalog_brand" :key="brand.id">
                <input type="checkbox" :value="brand.id" name="brand" @change="filterCatalog($event)" v-model="brand.active">
                <div class="caption">{{brand.title}}</div>
              </label>
            </div>
          </div>
          <div class="catalog_filter_items">
            <div class="catalog_filter_title">Rating</div>
            <div class="catalog_filter_item">
              <label class="input-rating" v-for="rating in catalog_rating" :key="rating.id">
                <input type="radio" :value="rating" name="rating" @change="filterCatalog($event)">
                <div class="caption">
                  <span class="star" v-for="n in 5" :key="n">
                    <svg-icon name="star" width="18" height="17" v-if="n <= rating" />
                    <svg-icon name="star-g" width="18" height="17" v-else />
                  </span>
                </div>
              </label>
            </div>
          </div>
        </div>
        <div class="clear-filter">
          <div class="btn btn-blue" v-on:click="clearFilter">CLEAR ALL FILTERS</div>
        </div>
      </div>
    </div>
    <div class="page-right-block">
      <div class="catalog-list-elements-block">
        <div class="row jc_space-between">
          <div class="title_block">{{catalog_i}} results found in 5ms</div>
          <div class="vid-and-sort-catalog row">
            <div class="sort">
              <v-select :options="['Default', 'Price', 'Rating']" :searchable="false" :clearable="false" v-model="sort_options"></v-select>
            </div>
            <div class="vid-btns row">
              <div class="vid-btn grid bg-box row ai_center jc_center" v-on:click="catalog_vid = 'grid'">
                <svg-icon name="grid" />
              </div>
              <div class="vid-btn list bg-box row ai_center jc_center" v-on:click="catalog_vid = 'list'">
                <svg-icon name="list" />
              </div>
            </div>
          </div>
        </div>
        <div class="catalog-searc">
          <input type="text" placeholder="Search hear" name="search" class="input-text" v-model="catalog_search" v-on:keyup="filterCatalog($event)">
          <button class="search-btn">
            <svg-icon name="search" />
          </button>
        </div>
        <div class="catalog-list-elements row" v-bind:class="catalog_vid" v-if="catalog_items_filter.length">
          <CatalogListElement v-for="catalog_item in catalog_pageOfItems" :key="catalog_item.id" :item="catalog_item" />
        </div>
        <div class="catalog-list-elements-empty" v-else>
          <p><b>Товары не найдены</b></p>
        </div>
      </div>
      <jw-pagination v-if="catalog_i > pageSize" :items="catalog_items_filter" @changePage="onChangePage" :pageSize="pageSize" :labels="customLabels"></jw-pagination>
    </div>
  </div>
</template>

<script>
const customLabels = {
    first: '<<',
    last: '>>',
    previous: '<',
    next: '>'
};

import axios from 'axios'
import CatalogListElement from '~/components/catalog/Catalog-list-element.vue'
import JwPagination from 'jw-vue-pagination';

import vSelect from 'vue-select';
import 'vue-select/dist/vue-select.css';

export default {
  components: {CatalogListElement, JwPagination, vSelect},

  data() {
    return {
      showFilter: false,
      sort_options: ["Default"],
      customLabels,
      pageSize: 9,
      catalog_items: [],
      catalog_i: 0,
      catalog_vid: "grid",
      catalog_price: {
        min: 1,
        max: "",
        valMin: 1,
        valMax: ""
      },
      catalog_range_active:[false, false, false, false, true],
      catalog_category: [],
      catalog_brand: [],
      catalog_rating: [5, 4, 3, 2, 1],
      catalog_search: "",
      catalog_pageOfItems: "",
      filters: {
        category: [],
        brand: [],
        rating: [],
        search: "",
      }
    }
  },
  computed: {
    catalog_items_filter: function(){
      return this.filterProductsByPrice(
        this.filterProductsByRating(
          this.filterProductsByBrand(
            this.filterProductsByCategory(
              this.filterProductsByName(this.catalog_items)
            )
          )
        )
      );
    },
  },
  watch: {
  },
  methods: {
    onChangePage(catalog_pageOfItems) {
      this.catalog_pageOfItems = catalog_pageOfItems;
    },

    clearFilter: function(){
      this.filters = {
        price: [],
        category: [],
        brand: [],
        rating: [],
        search: "",
      },
      this.catalog_price.valMin = 1;
      this.catalog_price.valMax = this.catalog_price.max;

      this.catalog_brand.forEach(function(item){
        item.active = false;
      })
      this.catalog_category.forEach(function(item){
        item.active = false;
      })

    },
    filterCatalog: function(event){
      if (event != ''){
        let prop = event.target.name,
          filters = this.filters;
        if(event.target.type == "checkbox"){
          if(event.target.checked){
            filters[prop].push(event.target.value);
          }else{
            filters[prop] = filters[prop].filter((n) => {return n != event.target.value});
          }
        }
        if(event.target.type == "radio"){
          filters[prop] = event.target.value;
        }
        if(event.target.type == "text"){
          filters[prop] = event.target.value;
        }
      }
      this.catalog_pageOfItems = this.catalog_items_filter;
    },
    filterProductsByCategory: function(products){
      if(this.filters.category.length){
        return products.filter(product => this.filters.category.includes(product.categoryId))
      }else{
        return products
      }
    },
    filterProductsByBrand: function(products){
      if(this.filters.brand.length){
        return products.filter(product => this.filters.brand.includes(product.brandId))
      }else{
        return products
      }
    },
    filterProductsByRating: function(products){
      if(this.filters.rating.length){
        return products.filter(product => parseInt(product.rating) >= this.filters.rating)
      }else{
        return products
      }
    },
    filterProductsByName: function(products) {
      if(this.filters.search){
        return products.filter(product => product.title.toLowerCase().includes(this.filters.search.toLowerCase()))
      }else{
        return products
      }
    },
    filterProductsByPrice: function(products) {
      if(this.catalog_price.valMin || this.catalog_price.valMax){
        return products.filter(product => (product.price >= this.catalog_price.valMin && product.price <= this.catalog_price.valMax))
      }else{
        return products
      }
    },
  },
  beforeMount() {
    let $this = this;

    // product
    axios.get(process.env.AXIOS_URL + '/product')
    .then(response => {
      this.catalog_items = response.data;
      this.catalog_items.forEach(function(item){
        if(item.price > $this.catalog_price.max){
          $this.catalog_price.max = item.price;
        }
      });
      $this.catalog_price.valMax = $this.catalog_price.max;
    }).catch(function (error){
      console.log(error);
    });

    // brand
    axios.get(process.env.AXIOS_URL + '/brand')
    .then(response => {
      this.catalog_brand = response.data;
      this.catalog_brand.forEach(function(item){
        item.active = false;
      })
    }).catch(function (error){
      console.log(error);
    });

    // category
    axios.get(process.env.AXIOS_URL + '/category')
    .then(response => {
      this.catalog_category = response.data;
      this.catalog_category.forEach(function(item){
        item.active = false;
      })
    }).catch(function (error){
      console.log(error);
    });
  },
  updated: function(){
    this.catalog_i = this.catalog_items_filter.length;
  }
}
</script>