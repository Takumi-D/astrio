<template>
  <div class="cart">

    <div v-if="productCart.length !== 0" class="header-cart flex-center">
      <div class="left-column-header">
        item
      </div>
      <div class="right-column-header flex-center">
        <div>price</div>
        <div>qty</div>
        <div>total</div>
      </div>
    </div>

    <div v-if="productCart.length !== 0" v-for="(product, index) in productCart" :key="index" class="item-cart flex-center">
      <div class="left-column flex-center">
        <img class="item-img" :src="image+(product.optionsItem.imageItem || product.image)" alt="product">
        <div class="wrapper-info">
          <div class="name">Brand {{product.brand}}/{{product.title}}</div>
          <div class="option">
            <div class="color">{{product.optionsItem.color}}</div>
            <div class="size">{{product.optionsItem.size}}</div>
          </div>
        </div>
      </div>
      <div class="right-column flex-center">
        <div class="wrapper-price">
          <div class="price">$ {{product.regular_price.value}}</div>
          <input @change="($event) => updatePrice(product, $event)" :value="product.qty" class="qty" type="number">
          <div class="total">$ {{product.total}}</div>
        </div>
        <button @click="() => deleteItem(product)" class="delete">DEL</button>
      </div>
    </div>
    <div class="empty" v-else>Корзина пуста</div>
    <div v-if="productCart.length !== 0" class="subtotal">Subtotal: $ {{totalPrice}}</div>

  </div>
</template>

<script>
export default {
  name: "Cart",
  props: {
    productCart: Array
  },
  data: () => ({
    image: "/src",
  }),
  methods: {
    updatePrice(product, $event){
      if ($event.target.value < 1) {
        $event.target.value = 1;
        this.$emit("updatePrice", {product: product, qty: 1});
      } else {
        this.$emit("updatePrice", {product: product, qty: $event.target.value});
      }
    },
    deleteItem(product){
      this.$emit("deleteItem", product);
    }
  },
   computed: {
     totalPrice(){
       let total = null;
       this.$props.productCart.forEach((element) => {
         total += +element.total;
       });
       return total?.toFixed(2) || 0;
     }
   }
}
</script>


<style scoped>
.cart{
  max-width: 1200px;
  width: 100%;
  margin: 0 auto;
  padding: 0 20px;
}

.item-cart{
  border-bottom: 1px solid #dfdfe1;
}

.flex-center{
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.item-img{
  height: 100px;
  width: 100px;
  margin-right: 10px;
}

.name{
  font-size: 18px;
  line-height: 21px;
}

.price{
  font-size: 16px;
  line-height: 19px;
}

.qty{
  width: 40px;
  margin: 0 30px;
}

.total{
  margin-right: 20px;
}

.left-column-header{
  width: 242px;
  text-align: center;
  font-weight: bold;
}

.right-column-header{
  width: 190px;
  margin-right: 72px;
  font-weight: bold;
}

.wrapper-price{
  display: flex;
}

.subtotal{
  text-align: end;
  margin-top: 10px;
  font-size: 19px;
  font-weight: bold;
}

.empty{
  text-align: center;
  font-size: 21px;
  margin-top: 20px;
}

@media(max-width: 620px){
  .item-cart{
    flex-flow: column;
    max-width: 240px;
    margin: 0 auto;
  }

  .header-cart{
    flex-flow: column;
  }

  .right-column-header{
    width: 100%;
    max-width: 220px;
  }

  .right-column{
    margin: 10px 0 10px 0;
    flex-flow: column;
    width: 100%;
    max-width: 240px;
  }

  .wrapper-price{
    justify-content: space-between;
    width: 100%;
  }

  .qty{
    margin: 0;
  }

  .total{
    margin: 0;
  }

  .delete{
    width: 100%;
    margin-top: 10px;
  }

  .right-column-header{
    margin: 10px 0 0 0;
  }

  .subtotal{
    text-align: center;
  }
}
</style>