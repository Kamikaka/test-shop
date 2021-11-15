<template>
  <div class="catalog-list page-mt row">
    <div class="page-left-block">
      <div class="catalog_filters" id="filter">
        <div class="title_block">Фильтр</div>
        <div class="catalog_filter bg-box">
          <div class="catalog_filter_items">
            <div class="catalog_filter_title">Multi Range</div>
            <div class="catalog_filter_item">
              <label class="input-radio">
                <input type="radio" value="10" name="price">
                <div class="caption">$10</div>
              </label>
              <label class="input-radio">
                <input type="radio" value="100" name="price">
                <div class="caption">$10 - $100</div>
              </label>
              <label class="input-radio">
                <input type="radio" value="500" name="price">
                <div class="caption">$100 - $500</div>
              </label>
              <label class="input-radio">
                <input type="radio" value="500" name="price">
                <div class="caption">$500</div>
              </label>
              <label class="input-radio">
                <input type="radio" value="" checked name="price">
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
                <input type="checkbox" :value="category.id" name="category" @change="filterCatalog($event)">
                <div class="caption">{{category.name}}</div>
              </label>
            </div>
          </div>
          <div class="catalog_filter_items">
            <div class="catalog_filter_title">Brand</div>
            <div class="catalog_filter_item">
              <label class="input-checkbox" v-for="brand in catalog_brand" :key="brand.id">
                <input type="checkbox" :value="brand.id" name="brand" @change="filterCatalog($event)">
                <div class="caption">{{brand.title}}</div>
              </label>
            </div>
          </div>
          <div class="catalog_filter_items">
            <div class="catalog_filter_title">Rating</div>
            <div class="catalog_filter_item">
              <label class="input-rating" v-for="rating in catalog_rating" :key="rating.id">
                <input type="radio" :value="rating" name="brand">
                <div class="caption">{{rating}}</div>
              </label>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="page-right-block">
      <div class="catalog-list-elements-block">
        <div class="row jc_space-between">
          <div class="title_block">{{catalog_i}} results found in 5ms</div>
          <div class="vid-and-sort-catalog row">
            <div class="sort">
              <select name="sort" id="">
                <option value="">Default</option>
                <option value="">Price</option>
              </select>
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
          <CatalogListElement v-for="catalog_item in catalog_items_filter" :key="catalog_item.id" :item="catalog_item" />
        </div>
        <div class="catalog-list-elements-empty" v-else>
          <p><b>Товары не найдены</b></p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import CatalogFilter from '~/components/Catalog-filter.vue'
import CatalogListElement from '~/components/catalog/Catalog-list-element.vue'
export default {
  components: { CatalogListElement, CatalogFilter},

  data() {
    return {
      catalog_items: [],
      catalog_items_filter: [],
      catalog_i: 0,
      catalog_vid: "grid",
      catalog_price: [],
      catalog_category: [],
      catalog_brand: [],
      catalog_rating: [5, 4, 3, 2, 1],
      catalog_search: "",
      filters: {
        price: [],
        category: [],
        brand: [],
        rating: [],
        search: "",
      }
    }
  },
  methods: {
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
      return this.catalog_items_filter
        // Фильтруем по категории
        .filter(element => {
          return this.filters.category != 0 || element.category_id == this.filters.category;
        })
 
        // // Фильтруем по брендам
        // .filter(catalog_items => {
        //   return this.selectBrand == 0 || product.brand == this.selectBrand;
        // })
 
        // // Фильтруем по полю поиска
        // .filter(procatalog_itemsduct => {
        //   return this.inputSearch == '' || product.good.toLowerCase().indexOf(this.inputSearch.toLowerCase()) !== -1;
        // });
    }
  },
  beforeMount() {
    // product
    axios.get('https://611a2bc9cbf1b30017eb5559.mockapi.io/product')
    .then(response => {
      this.catalog_items = response.data;
      this.catalog_items_filter = response.data;
    }).catch(function (error){
      console.log(error);
    });

    // brand
    axios.get('https://611a2bc9cbf1b30017eb5559.mockapi.io/brand')
    .then(response => {
      this.catalog_brand = response.data;
    }).catch(function (error){
      console.log(error);
    });

    // category
    axios.get('https://611a2bc9cbf1b30017eb5559.mockapi.io/category')
    .then(response => {
      this.catalog_category = response.data;
    }).catch(function (error){
      console.log(error);
    });
  },
  updated: function(){
    this.catalog_i = this.catalog_items_filter.length;
  }
}
</script>