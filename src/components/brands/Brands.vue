<template>
  <div class="brands">
    <div class="item-brands" @click="() => clickBrand('all')">All Brands</div>
    <div @click="() => clickBrand(brand)" class="item-brands" v-for="(brand, index) in brands" :key="index">
      {{brand.title}}
    </div>
  </div>
</template>

<script>
export default {
  name: "Brands",
  props: {
    brands: Object
  },
  methods: {
    clickBrand(value){
      this.$emit("clickBrand", value);
    }
  }
}
</script>

<style scoped>
.brands{
  display: flex;
  flex-flow: column;
  flex: 1 0 10%;
}

.item-brands{
  font-size: 18px;
  line-height: 21px;
  margin-bottom: 5px;
  width: fit-content;
  cursor: pointer;
}

.item-brands:last-child{
  margin-bottom: 0;
}

@media(max-width: 840px){
  .brands{
    flex: 1 0 15%;
  }
}

@media(max-width: 570px){
  .brands{
    flex: 1 0 20%;
  }
}

@media(max-width: 440px){
  .brands{
    flex: 1 0 30%;
  }
}
</style>