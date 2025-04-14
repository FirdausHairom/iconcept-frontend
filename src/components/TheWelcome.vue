<script setup>
import WelcomeItem from './WelcomeItem.vue'
import DocumentationIcon from './icons/IconDocumentation.vue'
import ToolingIcon from './icons/IconTooling.vue'
import EcosystemIcon from './icons/IconEcosystem.vue'
import CommunityIcon from './icons/IconCommunity.vue'
import SupportIcon from './icons/IconSupport.vue'
import { ref, onMounted } from 'vue'
import axios from 'axios'

const openReadmeInEditor = () => fetch('/__open-in-editor?file=README.md')

const error = ref(null)
const products = ref(null)

const getProduct = async () => {
  try {
    const response = await axios.get('http://localhost:8000/api/products')
    products.value = response.data
    console.log(products.value)
  } catch (err) {
    error.value = 'Failed to fetch products'
    console.error(error.value, err)
  }
}

onMounted(() => {
  getProduct()
})
</script>

<template>
 <div class="p-4">
    <h2 class="text-xl font-bold mb-4">Product List</h2>
    <table class="w-full table-auto border-collapse border border-gray-300">
      <thead class="bg-gray-100">
        <tr>
          <th class="border p-2">Title</th>
          <th class="border p-2">Price</th>
          <th class="border p-2">SKU</th>
          <th class="border p-2">Thumbnail</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="product in products" :key="product.id">
          <td class="border p-2">{{ product.title }}</td>
          <td class="border p-2">${{ product.price }}</td>
          <td class="border p-2">{{ product.sku }}</td>
          <td class="border p-2">
            <img :src="product.thumbnail" alt="thumb" class="h-12" />
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>
