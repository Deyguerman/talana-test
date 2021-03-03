<template>
  <q-dialog
    v-model="showDetail"
    persistent
    transition-show="scale"
    transition-hide="scale"
  >
    <q-card class="card-detail">
      <q-card-section horizontal>
        <q-img
          class="col-5"
          src="https://cdn.quasar.dev/img/parallax1.jpg"
          style="width: 200px"
        />

        <q-card-section class="row q-gutter-sm">
          <span class="col-12 text-bold">{{ item.name }}</span>
          <div class="col-12">
            <span class="text-bold">Precio: </span> {{ item.price }}
          </div>
          <div class="col-12">
            <span class="text-bold">Codigo Del Producto: </span>
            {{ item.code }}
          </div>
          <div class="col-12">
            <span class="text-bold">Cantidad: </span>
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
          </div>
          <div class="col-12">
            <span class="text-bold">Sub-Total: </span>
            {{ item.price * item.shop.quantity }}
          </div>
        </q-card-section>
      </q-card-section>

      <q-separator />

      <q-card-section class="text-justify">
        {{item.description}}
      </q-card-section>

      <q-separator />

      <q-card-actions class="flex justify-between">
        <q-btn outline color="gray" @click="keepBuying">
          Seguir Comprando
        </q-btn>
        <q-btn color="primary" @click="addToCard">
          Agregar al carro
        </q-btn>
      </q-card-actions>
    </q-card>
  </q-dialog>
</template>

<script>
export default {
  name: "ProductDetail",
  methods: {
    addToCard() {
      this.$root.$emit("addToCard", this.item);
    },
    keepBuying() {
      this.$root.$emit("closeModalDetail");
    }
  },
  props: {
    showDetail: {
      type: Boolean,
      default: false
    },
    item: {
      type: Object,
      default: () => {},
      required: true
    }
  }
};
</script>

<style lang="scss" scoped>
.card-detail {
  width: 100%;
  max-width: 550px;
}
</style>
