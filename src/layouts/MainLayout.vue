<template>
  <q-layout view="hHh lpR fFf">
    <q-header elevated class="bg-white text-dark">
      <q-toolbar>
        <q-btn dense flat round icon="menu" @click="left = !left" />

        <q-toolbar-title>
          <q-avatar>
            <img src="../assets/img/talana-logo.png" />
          </q-avatar>
          Pet Store
        </q-toolbar-title>

        <q-btn color="primary" icon="shopping_cart">
          <q-menu>
            <div class="row no-wrap q-pa-md">
              <div class="column">
                <div class="text-h6 q-mb-md">Shopping Car</div>
              </div>
            </div>
          </q-menu>
        </q-btn>
      </q-toolbar>
    </q-header>

    <q-drawer
      v-model="drawer"
      show-if-above
      :width="200"
      :breakpoint="500"
      bordered
      content-class="bg-grey-3"
    >
      <q-scroll-area class="fit">
        <q-list>
          <template v-for="(menuItem, index) in categoryList">
            <q-item
              :key="index"
              clickable
              :active="menuItem.name === 'Outbox'"
              v-ripple
              @click="setCategory(menuItem)"
            >
              <!-- <q-item-section avatar>
                <q-icon :name="menuItem.icon" />
              </q-item-section> -->
              <q-item-section>
                {{ menuItem.name }}
              </q-item-section>
            </q-item>
            <!-- <q-separator :key="'sep' + index" v-if="menuItem.separator" /> -->
          </template>
        </q-list>
      </q-scroll-area>
    </q-drawer>

    <q-page-container>
      <router-view />
    </q-page-container>
  </q-layout>
</template>

<script>
export default {
  data() {
    return {
      drawer: false,
      mobileData: true,
      bluetooth: false,
      categoryList: []
    };
  },
  created() {
    this.$axios
      .get("http://sva.talana.com:8000/api/product-category/")
      .then(result => {
        this.categoryList = result.data;
        this.$store.dispatch("global/setCategory", this.categoryList[0]);
      });

    this.$axios.get("http://sva.talana.com:8000/api/product/").then(async result => {
      // this.categoryList = result.data;
      const data = await result.data.map(item => {
        return {
          ...item,
          shop: { quantity: 0 }
        };
      });
      this.$store.dispatch("global/setProductList", data);
    });
  },
  methods: {
    setCategory(category) {
      this.$store.dispatch("global/setCategory", category);
    }
  }
};
</script>
