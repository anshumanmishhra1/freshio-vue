<template>
  <section class="mt-4 items-center">
    <h1 class="hero-text-color-left text-2xl ml-8 font-bold">
      Feature Products
    </h1>

    <!-- Product Grid -->
    <div
      id="productGrid"
      class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6 p-4 ml-4"
    >
      <!-- Product Card -->
      <div
        v-for="(item, index) in products"
        :key="item.id"
        class="border rounded-xl shadow-md p-4 flex flex-col items-center"
      >
        <img
          :src="item.img"
          alt="Product Image"
          class="rounded-lg mb-4 w-full h-48 object-cover"
        />
        <h2 class="font-bold text-lg">{{ item.title }}</h2>
        <p class="text-gray-600">{{ item.price }}</p>
        <button
          @click="addToCart(item)"
          class="mt-2 px-4 py-2 bg-green-600 text-white rounded-lg hover:bg-green-700"
        >
          Add to Cart
        </button>
      </div>
    </div>

    <!-- Cart Count -->
    <p class="mt-4 ml-8 font-bold">
      Items in Cart: {{ cart.length }}
    </p>

    <!-- Loading message -->
    <p v-if="loading" class="text-center text-gray-500 mt-4">
      Loading products...
    </p>
  </section>
</template>

<script setup>
import { ref, onMounted } from "vue";
import axios from "axios";

// Reactive arrays
const products = ref([]);
const cart = ref([]);
const loading = ref(true);

// Fetch products from GitHub
onMounted(async () => {
  try {
    const response = await axios.get(
      "https://raw.githubusercontent.com/anshumanmishhra1/products.json/main/item.json"
    );
    products.value = response.data;
    console.log("Products fetched:", products.value);
  } catch (error) {
    console.error("Error fetching products:", error);
  } finally {
    loading.value = false;
  }
});

// Add product to cart
const addToCart = (item) => {
  cart.value.push(item);
  console.log("Cart Items:", cart.value);
};
</script>

<style scoped>
#productGrid div:hover {
  transform: translateY(-5px);
  transition: transform 0.3s ease;
}
</style>
