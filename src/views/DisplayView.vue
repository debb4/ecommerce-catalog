<template>
  <div class="ProductView">
    <ProductDisplay />
  </div>
</template>

<script>
// @ is an alias to /src
// import DisplayComponent from "@/components/DisplayComponent.vue";
import ProductDisplay from "@/components/ProductDisplay.vue";

export default {
  name: "ProductView",
  components: {
    ProductDisplay,
  },
};
</script>
