<template>
  <div :class="style">
    <img @click="showMain" class="logo" src="../../images/logo.png" alt="logo">
    <div v-show="showIconCard" class="wrapper-cart">
      <div v-show="productCart.length !== 0" class="amount">{{productCart.length}}</div>
      <img @click="showCard" class="cart-img" src="../../images/cart.svg" alt="cart">
    </div>
  </div>
</template>

<script>
export default {
  name: "Header",
  data: () => ({
    showIconCard: true,
    style: {
      header: true,
      none: false
    }
  }),
  props: {
    productCart: Array
  },
  methods: {
    showCard(){
      this.$emit("showCard");
      this.style.none = true;
      this.showIconCard = false;
    },
    showMain(){
      this.$emit("showMain");
      this.style.none = false;
      this.showIconCard = true;
    }
  }
}
</script>

<style scoped>
.header{
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 20px 0;
}
.logo{
  cursor: pointer;
}

.wrapper-cart{
  position: relative;
  width: 24px;
  height: 24px;
  cursor: pointer;
}

.amount{
  position: absolute;
  right: -4px;
  top: -4px;
  font-size: 11px;
  border-radius: 50%;
  background: red;
  width: 15px;
  height: 15px;
  z-index: 1;
  display: flex;
  justify-content: center;
  align-items: center;
  color: #ffffff;
}

.cart-img{
  width: 100%;
  height: 100%;
}
.none{
  justify-content: center;
}
</style>