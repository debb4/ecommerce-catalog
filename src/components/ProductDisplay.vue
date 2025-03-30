<template>
  <div class="full-width-background" :class="productBackgroundClass">
    <div class="content-wrapper">
      <div class="container-display">
        <div v-if="loading" class="loading-overlay">
          <div class="loader"></div>
        </div>
        <!--error state-->
        <div v-else-if="error" class="error-message">
          <h2>
            <h2>⚠️ Error Loading Product</h2>
            <p>{{ error }}</p>
            <button @click="fetchProduct">Try Again</button>
          </h2>
        </div>

        <!-- Unavailable Product State -->
        <div v-else-if="!productValid" class="unavailable-section">
          <div class="unavailable-content">
            <div class="unavailable-img">
              <!-- <img src="../assets/sad-face.png" alt="" /> -->
              <div class="unavailable-inner">
                <p class="unavailable-text">
                  This product is unavailable to show
                </p>
                <button class="next-button-unavailable" @click="nextProduct">
                  Next product
                </button>
              </div>
            </div>
          </div>
        </div>

        <!--Main Content-->
        <div v-else class="inner-display">
          <!-- Product Image-->
          <div class="container-img">
            <img
              class="product-img"
              :src="product.image"
              alt="product.title"
              v-if="productValid"
            />
          </div>

          <!--Product Description-->
          <div class="container-desc-display">
            <h1 class="title-display">
              {{ product.title || "Loading Product..." }}
            </h1>
            <!--Justify between-->
            <div class="section-rating" v-if="productValid">
              <div class="category-section">{{ formattedCategory }}</div>

              <div class="rating">
                <span class="nummer-rate"
                  >{{ product.rating?.rate.toFixed(1) || "0" }}/5</span
                >
                <div class="dot-rating">
                  <span
                    v-for="i in 5"
                    :key="i"
                    :class="{
                      filled: i <= Math.round(product.rating?.rate || 0),
                    }"
                  ></span>
                </div>
              </div>
            </div>
            <div class="divider"></div>
            <!--Product-->
            <div class="desc-product" v-if="productValid">
              <p>
                {{ product.description }}
              </p>
            </div>
            <div class="divider"></div>
            <h3 class="price" v-if="productValid">
              ${{ product.price?.toFixed(2) }}
            </h3>
            <div class="button-container">
              <button class="buy-button" :disabled="!productValid">
                Buy Now
              </button>
              <button class="next-button" @click="nextProduct">
                Next Product
              </button>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "ProductView",
  data() {
    return {
      product: {},
      currentId: 1,
      maxProducts: 20,
      loading: false,
      error: null,
    };
  },
  computed: {
    productValid() {
      return ["men's clothing", "women's clothing"].includes(
        this.product.category
      );
    },
    formattedCategory() {
      return this.product.category === "men's clothing"
        ? "Men's Section"
        : this.product.category === "women's clothing"
        ? "Women's Section"
        : "";
    },
    productBackgroundClass() {
      if (!this.productValid) return "unavailable-section";
      return this.product.category === "men's clothing"
        ? "men-section"
        : "women-section";
    },
  },
  methods: {
    async fetchProduct() {
      this.loading = true;
      this.error = null;

      try {
        const response = await axios.get(
          `https://fakestoreapi.com/products/${this.currentId}`
        );

        if (
          ["men's clothing", "women's clothing"].includes(
            response.data.category
          )
        ) {
          this.product = response.data;
        } else {
          this.product = { category: response.data.category };
        }
      } catch (err) {
        console.error("Error fetching product:", err);
        this.error = err.message;
      } finally {
        this.loading = false;
      }
    },
    nextProduct() {
      this.currentId =
        this.currentId < this.maxProducts ? this.currentId + 1 : 1;
      this.fetchProduct();
    },
  },
  mounted() {
    this.fetchProduct();
  },
};
</script>

<style></style>
