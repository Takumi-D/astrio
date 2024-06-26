<template>
  <div class="tile">
    <Card v-if="products.length !== 0" :product="product" v-for="(product, index) in products"
          @clickCart="clickCart"
          :key="product.id"
    />
    <div v-else>Такого товара нет</div>
  </div>
</template>

<script>
import Card from "../card";
export default {
  name: "Tile",
  props: {
    products: Object
  },
  components: {
    Card
  },
  methods: {
    clickCart(product){
      this.$emit("clickCart", product);
    }
  }
}
</script>

<style scoped>
.tile{
  display: flex;
  justify-content: center;
  flex-wrap: wrap;
  flex: 1 0 90%;
}

@media(max-width: 840px){
  .tile{
    flex: 1 0 85%;
  }
}

@media(max-width: 570px){
  .tile{
    flex: 1 0 80%;
  }
}

@media(max-width: 440px){
  .tile{
    flex: 1 0 70%;
  }
}
</style>