<template>
  <div class="container">
    <div class="row">
      <div class="col-6 offset-3 pt-3 card mt-5 shadow">
        <div class="card-body">
          <h3>Ürün Çıkışı</h3>
          <hr />
          <div class="form-group">
            <label>Ürün Adı</label>
            <select
              class="form-control"
              v-model="selectedProduct"
              @change="productSelected"
            >
              <option disabled selected>Lütfen bir ürün seçiniz...</option>
              <option
                :disabled="product.count == 0"
                :value="product.key"
                v-for="product in getProducts"
                >{{ product.title }}</option
              >
            </select>
          </div>
          <transition name="fade" mode="out-in">
            <div class="card mb-2 border border-danger" v-if="product !== null">
              <div class="card-body">
                <div class="row">
                  <div class="col-12 text-center">
                    <div class="mb-3">
                      <span class="badge badge-info"
                        >Stok : {{ product.count }}</span
                      >
                      <span class="badge badge-primary"
                        >Fiyat : {{ product.price | currency }}</span
                      >
                    </div>
                    <p class="border border-warning p-2 text-secondary">
                      {{ product.description }}
                    </p>
                  </div>
                </div>
              </div>
            </div>
          </transition>
          <div class="form-group">
            <label>Adet</label>
            <input
              type="text"
              v-model="product_count"
              class="form-control"
              placeholder="Ürün adetini giriniz.."
            />
          </div>
          <hr />
          <button @click="save" class="btn btn-primary">Kaydet</button>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import { mapGetters } from "vuex";
export default {
  data() {
    return {
      selectedProduct: null,
      product: null,
      product_count: null
    };
  },
  computed: mapGetters(["getProducts"]),
  methods: {
    productSelected() {
      this.product = this.$store.getters.getProduct(this.selectedProduct)[0];
    },
    save() {
      let product = {
        key: this.selectedProduct,
        count: this.product_count
      };
      this.$store.dispatch("sellProduct", product);
    },
    beforeRouteLeave(to, from, next) {
      if (
        (this.selectedProduct !== null ||
          this.product_count > 0 )
      ) {
        if (
          confirm(
            "kaydedilmemiş değişiklikler var. Yine de çıkmak istiyormusunuz ?"
          )
        ) {
          next();
        } else {
          next(false);
        }
      } else {
        next();
      }
    }
  }
};
</script>
<style scoped>
.border-danger {
  border-style: dashed !important;
}
</style>
