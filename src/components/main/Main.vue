<template>
  <div class="main">
    <Brands @clickBrand="clickBrand" :brands="brands"/>
    <Tile @clickCart="clickCart" :products="products"/>
  </div>
</template>

<script>
import Tile from "../tile";
import Brands from "../brands";
export default {
  name: "Main",
  props: {
    products: Object,
    brands: Object
  },
  components: {
    Tile,
    Brands
  },
  methods: {
    clickBrand(value){
      this.$emit("clickBrand", value);
    },
    clickCart(product){
      this.$emit("clickCart", product);
    }
  }
}
</script>

<style scoped>
.main{
  display: flex;
}
</style>