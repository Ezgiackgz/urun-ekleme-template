<template>
  <div v-if="productList.length > 0">
    <h3 class="text-center">Eklenen Ürünlerin Listesi</h3>
    <app-product v-for="product in productList" v-bind:key="product">
      <img
        class="card-img-top"
        :src="product.selectedImage"
        :alt="product.title"
      />
      <div class="card-body">
        <h5 class="card-title">{{ product.title }}</h5>
        <small> <strong>Adet : </strong> {{ product.count }} </small>
        <br />
        <small> <strong>Fiyat : </strong> {{ product.price }} </small>
        <br />
        <small> <strong>Tutar : </strong> {{ product.totalPrice }} </small>
        <button class="btn btn-danger" @click="deleteProducts(index)">
          Sil
        </button>
        <button class="btn btn-success" >
          Düzenle
        </button>
      </div>
    </app-product>
  </div>
</template>
<script>
import { eventBus } from "../main";
import Product from "./Product.vue";
export default {
  components: {
    appProduct: Product,
  },
  data() {
    return {
      productList: [],
    };
  },
  methods:{
    deleteProducts(index){
      this.productList.splice(index,1)
    }
  },
  created() {
    eventBus.$on("productAdded", (product) => {
      if (this.productList.length <= 10) {
        this.productList.push(product);
        eventBus.$emit("proggressBarUpdate", this.productList.length);
      } else {
          this.$swal.fire({
          text: "En fazla 10 ürün Yükleyebilirsiniz",
          icon: "error",
          confirmButtonText: "Tamam",
          allowOutsideClick: false,
          allowEscapeKey: false,
        });
      }
    });
  },
 
};
</script>
