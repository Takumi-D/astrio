<template>
  <div class="app">
    <Header :productCart="productCart"
            @showCard="showCard"
            @showMain="showMain"
    />
    <Main v-if="showComponents"
          @clickBrand="sortBrand"
          @click-cart="pushCart"
          :brands="brands"
          :products="sortProducts"/>

    <Cart v-else
          :productCart="productCart"
          @updatePrice="updatePrice"
          @deleteItem="deleteItem"
    />
  </div>
</template>

<script>
import products from "../../service/service-products.js";
import brands from "../../service/service-brands.js";

import Main from "../main";
import Header from "../header";
import Cart from "../cart";
export default {
  name: "App",
  data: () => ({
    products: products,
    sortProducts: [...products],
    productCart: [],
    brands: brands,
    showComponents: true
  }),
  components: {
    Main,
    Header,
    Cart
  },
  methods: {
    sortBrand(value){
      if(value === "all") {
        this.sortProducts = products;
      } else {
        this.sortProducts = products.filter((product) => product.brand === value.id);
      }
    },
    comparesColorAndSize(product, options){
      return product.optionsItem?.size === options?.size && product.optionsItem?.color === options?.color;
    },
    comparesIDAndUndefined(value, product, options){
      return value.id === product.id && options !== undefined;
    },
    pushCart({productItem, optionsItem}){
      if(this.productCart.length === 0){
        this.productCart = [...this.productCart, {...productItem, qty: 1, total: productItem.regular_price.value, optionsItem: {...optionsItem}}];
      } else {
        const indexElement = this.productCart.findIndex((element) => element.id === productItem.id);
        const indexElOptions = this.productCart.findIndex((element) => element.id === productItem.id && this.comparesColorAndSize(element, optionsItem));
        if(indexElement !== -1 && indexElOptions !== -1) {
          this.productCart.forEach((value) => {
            if(this.comparesIDAndUndefined(value, productItem, optionsItem)){
              this.productCart[indexElement].qty++;
              this.productCart[indexElement].total = +value.total + +value.regular_price.value;
            } else if(this.comparesIDAndUndefined(value, productItem, optionsItem) && this.comparesColorAndSize(value, optionsItem)) {
              this.productCart[indexElOptions].qty++;
              this.productCart[indexElOptions].total = +value.total + +value.regular_price.value;
            }
          });
        } else {
          this.productCart = [...this.productCart, {...productItem, qty: 1, total: productItem.regular_price.value, optionsItem: {...optionsItem}}];
        }
      }
    },
    updatePrice({product, qty}){
      const indexElement = this.productCart.findIndex((element) => element.id === product.id);
      const indexElOptions = this.productCart.findIndex((element) => element.id === product.id && this.comparesColorAndSize(element, product.optionsItem));
      this.productCart.forEach((value) => {
        if(value.id === product.id && Object.keys(product.optionsItem).length === 0){
          this.productCart[indexElement].qty = +qty;
          this.productCart[indexElement].total = (+value.regular_price.value * +qty).toFixed(2);
        } else if(this.comparesIDAndUndefined(value, product, product.optionsItem) && this.comparesColorAndSize(value, product.optionsItem)){
          this.productCart[indexElOptions].qty = +qty;
          this.productCart[indexElOptions].total = (+value.regular_price.value * +qty).toFixed(2);
        }
      });
    },
    deleteItem(product){
      this.productCart = this.productCart.filter((value) => {
        if(this.comparesIDAndUndefined(value, product, product.optionsItem) && this.comparesColorAndSize(value, product.optionsItem)){
          return false;
        } else if(value.id === product.id && Object.keys(product.optionsItem).length === 0) {
          return false;
        } else {
          return true;
        }
      });
    },
    showCard(){
      this.showComponents = false;
    },
    showMain(){
      this.showComponents = true;
    }
  }
}
</script>

<style>
.app{
  max-width: 1200px;
  width: 100%;
  margin: 0 auto;
  padding: 0 20px;
}
</style>
