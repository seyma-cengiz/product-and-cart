<template>
  <aside class="cart-container">
    <div class="cart">
      <h1 class="cart-title spread">
        <span>
          Cart
          <i class="icofont-cart-alt icofont-1x"></i>
        </span>
        <button class="cart-close" @click="toggleSidebar">&times;</button>
      </h1>

      <div class="cart-body">
        <table class="cart-table">
          <thead>
            <tr>
              <th><span class="sr-only">Product Image</span></th>
              <th>Product</th>
              <th>Price</th>
              <th>Qty</th>
              <th>Total</th>
              <th><span class="sr-only">Actions</span></th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(item, index) in cart" :key="index">
              <td><i :class="getIcon(item.id)"></i></td>
              <td>{{ item.name }}</td>
              <td>${{ item.price }}</td>
              <td class="center">{{ item.quantity }}</td>
              <td>${{ getItemTotal(item) }}</td>
              <td class="center">
                <button
                  class="btn btn-light cart-remove"
                  @click="removeItem(item.id)"
                >
                  &times;
                </button>
              </td>
            </tr>
          </tbody>
        </table>

        <p class="center" v-if="!cart.length"><em>No items in cart</em></p>
        <div class="spread">
          <span><strong>Total:</strong> ${{ calculateCart }}</span>
          <button class="btn btn-light">Checkout</button>
        </div>
      </div>
    </div>
  </aside>
</template>

<script>
export default {
  props: ['toggleSidebar', 'cart', 'inventory', 'removeItem'],
  computed: {
    calculateCart() {
      return this.cart
        .reduce((sum, item, i) => {
          return (sum += +this.getItemTotal(item))
        }, 0)
        .toFixed(2)
    }
  },
  methods: {
    getItemTotal(item) {
      return (item.quantity * item.price).toFixed(2)
    },
    getIcon(id) {
      var item = this.inventory.find((x) => x.id == id)
      return 'icofont-3x icofont-' + item.icon
    }
  }
}
</script>
