<template>
  <div class="catalog-list row">
    <CatalogFilter />
    <div class="catalog-list-element">
      <div class="row">
        <div class="title_block">7,618 results found in 5ms</div>
      </div>
      <CatalogListElement :key="catalog_items.id" v-for="catalog_item in catalog_items" />
    </div>
    
  </div>
</template>

<script>
import CatalogFilter from '~/components/Catalog-filter.vue'
import CatalogListElement from '~/components/catalog/Catalog-list-element.vue'
export default {
  components: { CatalogListElement, CatalogFilter },

  data() {
    return {
      catalog_items: []
    }
  },

  async fetch() {
    this.catalog_items = await fetch(
      'https://611a2bc9cbf1b30017eb5559.mockapi.io/product'
    ).then(res => res.json())
  }
}
</script>
