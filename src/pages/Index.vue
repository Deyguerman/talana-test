<template>
  <q-page class="q-pa-md">
    <h4 class="q-my-sm">{{ category.name }}</h4>
    <div class="q-pa-md row items-start q-gutter-md" v-if="items.length > 0">
      <div v-for="(item, key) in items" :key="key">
        <Product :item="{...item}" />
      </div>
    </div>
    <div class="q-pa-md row items-start q-gutter-md text-center" v-if="items.length == 0 && category.id">
      <h5 class="q-mx-auto">No tenemos productos disponibles</h5>
    </div>
  </q-page>
</template>

<script>
import Product from "../components/Product";
export default {
  name: "PageIndex",
  data() {
    return {
      items: []
    };
  },
  methods: {},
  watch: {
    category: function(category) {
      const list = [...this.productList]
      this.items = list.filter(
        item => item.category.id == category.id
      );
    }
  },
  computed: {
    category: {
      get: function() {
        return this.$store.getters["global/getCategory"];
      }
    },
    productList: {
      get: function() {
        return this.$store.getters["global/getProductList"];
      }
    }
  },
  components: {
    Product
  }
};
</script>

<style lang="scss" scoped>
.card {
  width: 100%;
  max-width: 250px;
}
</style>
