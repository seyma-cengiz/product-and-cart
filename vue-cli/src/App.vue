<template>
  <header class="top-bar spread">
    <nav class="top-bar-nav">
      <router-link to="/" class="top-bar-link">
        <i class="icofont-spoon-and-fork"></i>
        <span>Home</span>
      </router-link>
      <router-link to="products" class="top-bar-link">
        <span>Products</span>
      </router-link>
      <router-link to="past-orders" class="top-bar-link">
        <span>Past Orders</span>
      </router-link>
    </nav>
    <a href="#" class="top-bar-cart-link" @click="toggleSidebar">
      <i class="icofont-cart-alt icofont-1x"></i>
      <span>Cart ({{ getCartItemCount }})</span>
    </a>
  </header>

  <router-view :inventory="inventory" :addToCart="addToCart" />

  <Sidebar
    v-if="showSidebar"
    :toggle="toggleSidebar"
    :cart="cart"
    :inventory="inventory"
    :remove="removeItem"
  />
</template>

<script>
import Sidebar from '@/components/Sidebar.vue'
import food from './food.json'

export default {
  data() {
    return {
      showSidebar: false,
      inventory: food,
      cart: []
    }
  },
  computed: {
    getCartItemCount() {
      return this.cart.reduce((sum, item, i) => {
        return (sum += item.quantity)
      }, 0)
    }
  },
  methods: {
    toggleSidebar() {
      this.showSidebar = !this.showSidebar
    },
    addToCart(productName, id) {
      var inventoryItem = this.inventory.find((x) => x.id == id)
      var cartItem = this.cart.find((x) => x.id == id)
      if (!cartItem)
        this.cart.push({
          id: id,
          name: productName,
          quantity: inventoryItem.quantity,
          price: inventoryItem.price.USD
        })
      else cartItem.quantity += inventoryItem.quantity

      inventoryItem.quantity = 0
    }
  },
  components: {
    Sidebar
  }
}
</script>
