<template>
  <div>
    <q-card class="card">
      <div @click="showModalDetail()">
        <q-img :src="item.photo" style="height: 200px"/>

        <q-card-section>
          <div
            fab
            class="absolute q-px-lg text-white"
            style="top: -10px; left: 0px; transform: translateY(-50%); background-color: rgba(0,0,0,0.8)"
          >
            {{item.stock > 0 ? 'Disponible' : 'Sin Stock'}}
          </div>

          <div class="row no-wrap items-center">
            <div class="col text-h6 ellipsis">
              {{item.name}}
            </div>
          </div>

          <q-rating v-model="stars" readonly :max="5" size="32px" />
        </q-card-section>
      </div>

      <q-separator />

      <q-card-actions>
        <div>
          <q-btn
            padding="xs"
            outline
            color="primary"
            icon="remove"
            :disabled="item.shop.quantity == 0"
            @click="--item.shop.quantity"
          />
          <span class="q-px-md">{{ item.shop.quantity }}</span>
          <q-btn
            padding="xs"
            outline
            color="primary"
            icon="add"
            @click="++item.shop.quantity"
          />
        </div>

        <div class="q-ml-auto">
          <q-btn padding="xs" color="primary" icon="shopping_cart" />
        </div>
      </q-card-actions>
    </q-card>
    <ProductDetail :showDetail="showDetail" :item="item" />
  </div>
</template>

<script>
import ProductDetail from "./ProductDetail";
export default {
  name: "Product",
  data() {
    return {
      stars: 4,
      showDetail: false
    };
  },
  methods: {
    addToCard(item) {
      console.log("Add To card");
      if (this.showDetail) {
        this.showModalDetail();
      }
    },
    showModalDetail() {
      this.showDetail = !this.showDetail;
      if (this.showDetail) {
        this.$root.$on("addToCard", this.addToCard);
        this.$root.$on("closeModalDetail", this.showModalDetail);
      } else {
        this.$root.$off("addToCard", this.addToCard);
        this.$root.$off("closeModalDetail", this.showModalDetail);
      }
    }
  },
  props: {
    item: {
      type: Object,
      default: () => {},
      required: true
    }
  },
  components: {
    ProductDetail
  }
};
</script>

<style lang="scss" scoped>
.card {
  width: 250px;
}
</style>
