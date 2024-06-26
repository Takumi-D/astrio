<template>
  <div @click="() => clickCart(product)" class="card">
    <img class="img" :src="image+(imageItem || product.image)" alt="product">
    <div class="content">
      <div class="title">{{product.title}}</div>
      <div class="brand">Бренд {{product.brand}}</div>
      <div class="price">$ {{product.regular_price.value}}</div>
      <div
          v-for="options in product.configurable_options"
          :class="options.attribute_code"
      >
        <div
            v-for="value in options.values"
             :class="{' item-options ': true,  [value.label]: true}"
             @click="($event) => configurableOptions(value, options, $event)"
        >
          {{value.label}}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Card",
  data: () => ({
    image: "/src",
    size: null,
    color: null,
    imageItem: null,
    target: null
  }),
  props: {
    product: Object
  },
  methods: {
    clickCart(productItem){
      if(this.size && this.color){
        this.$emit("click-cart", {productItem: {...productItem}, optionsItem: {size: this.size, color: this.color, imageItem: this.imageItem}});
      } else if(!productItem.configurable_options){
        this.$emit("click-cart", {productItem: {...productItem}});
      } else {
        alert("Выбирете размер и цвет товара");
      }
    },
    configurableOptions(value, options, $event){
      $event.stopPropagation();
      if(options.label === "Size"){
        this.size = value.label;
        for(value of $event.target.parentNode.children){
          if(value.classList.contains("active")){
            value.classList.remove("active");
          }
        }
        $event.target.classList.add("active");
      } else {
        this.color = value.label;
        this.imageItem = `/images/conf/${value.label}.png`;
        for(value of $event.target.parentNode.children){
          if(value.classList.contains("active")){
            value.classList.remove("active");
          }
        }
        $event.target.classList.add("active");
      }
    }
  }
}
</script>

<style scoped>
.card{
  max-width: 250px;
  width:100%;
  cursor: pointer;
  border-bottom: 1px solid #dfdfe1;
  margin-bottom: 10px;
  margin-right: 10px;
}
.card:hover{
  box-shadow: 0 5px 25px rgb(30 31 33 / 12%);
  border-top-color: transparent;
}
.img{
  max-width: 250px;
  max-height: 250px;
  width: 100%;
  height: 100%;
}

.content{
  padding: 0 10px;
}

.title{
  font-size: 18px;
  line-height: 21px;
  font-weight: bold;
}

.brand{
  font-size: 14px;
  line-height: 18px;
}

.price{
  font-size: 14px;
  line-height: 18px;
}
.color{
  display: flex;
}

.size{
  display: flex;
}

.item-options{
  display: flex;
  justify-content: center;
  align-items: center;
  width: 40px;
  height: 20px;
  margin: 5px;
}

.item-options:hover{
  border: solid 2px sandybrown;
}

.item-options:first-child{
  margin-left: 0;
}

.item-options:last-child{
  margin-right: 0;
}

.Black{
  background-color: black;
  color: black
}

.Blue{
  background-color: blue;
  color: blue
}
.Red{
  background-color: red;
  color: red
}

.active{
  border: solid 2px sandybrown;
}

@media(max-width: 440px){
  .card{
   height: fit-content;
  }
}
</style>