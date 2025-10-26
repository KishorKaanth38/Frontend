<template>
  <div>
    <h2>Available Lessons</h2>

    <!-- Sort controls -->
    <div class="controls">
      <label>Sort by:</label>
      <select v-model="sortKey">
        <option value="subject">Subject</option>
        <option value="location">Location</option>
        <option value="price">Price</option>
        <option value="spaces">Spaces</option>
      </select>
      <button @click="toggleSortOrder">
        Sort: {{ sortOrder === 1 ? 'Ascending' : 'Descending' }}
      </button>
    </div>

    <!-- Lesson list -->
    <div class="lesson-list">
      <div class="lesson-card" v-for="lesson in sortedLessons" :key="lesson.id">
        <h3>{{ lesson.subject }}</h3>
        <p><strong>Location:</strong> {{ lesson.location }}</p>
        <p><strong>Price:</strong> £{{ lesson.price }}</p>
        <p><strong>Spaces:</strong> {{ lesson.spaces }}</p>
        <button :disabled="lesson.spaces === 0" @click="addToCart(lesson)">
          Add to Cart
        </button>
      </div>
    </div>

    <!-- Cart -->
    <div class="cart">
      <h2>Shopping Cart</h2>
      <div v-if="cart.length === 0">Your cart is empty.</div>
      <div v-else>
        <div v-for="item in cart" :key="item.id">
          {{ item.subject }} – £{{ item.price }} (x{{ item.quantity }})
          <button @click="removeFromCart(item)">Remove</button>
        </div>
        <p><strong>Total: £{{ totalPrice }}</strong></p>

        <!-- Checkout -->
        <div class="checkout">
          <h3>Checkout</h3>
          <label>Name:</label>
          <input v-model="customerName" type="text" />
          <label>Phone:</label>
          <input v-model="customerPhone" type="text" />
          <button :disabled="!isFormValid" @click="checkout">Submit Order</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      lessons: [
        { id: 1, subject: 'Math', location: 'Hendon', price: 100, spaces: 5 },
        { id: 2, subject: 'Science', location: 'Colindale', price: 80, spaces: 2 },
        { id: 3, subject: 'Art', location: 'Brent Cross', price: 90, spaces: 6 },
        { id: 4, subject: 'English', location: 'Golders Green', price: 95, spaces: 7 }
      ],
      cart: [],
      sortKey: 'subject',
      sortOrder: 1,
      customerName: '',
      customerPhone: ''
    }
  },
  computed: {
    sortedLessons() {
      return [...this.lessons].sort((a, b) => {
        if (a[this.sortKey] < b[this.sortKey]) return -1 * this.sortOrder
        if (a[this.sortKey] > b[this.sortKey]) return 1 * this.sortOrder
        return 0
      })
    },
    totalPrice() {
      return this.cart.reduce((sum, i) => sum + i.price * i.quantity, 0)
    },
    isFormValid() {
      const nameOk = /^[A-Za-z\s]+$/.test(this.customerName)
      const phoneOk = /^[0-9]+$/.test(this.customerPhone)
      return nameOk && phoneOk && this.cart.length > 0
    }
  },
  methods: {
    toggleSortOrder() {
      this.sortOrder *= -1
    },
    addToCart(lesson) {
      const item = this.cart.find(i => i.id === lesson.id)
      if (item) item.quantity++
      else this.cart.push({ ...lesson, quantity: 1 })
      lesson.spaces--
    },
    removeFromCart(item) {
      const lesson = this.lessons.find(l => l.id === item.id)
      lesson.spaces += item.quantity
      this.cart = this.cart.filter(i => i.id !== item.id)
    },
    checkout() {
      alert(`Order placed for ${this.customerName}!`)
      this.cart = []
      this.customerName = ''
      this.customerPhone = ''
    }
  }
}
</script>

<style scoped>
.lesson-list {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(220px, 1fr));
  gap: 20px;
}
.lesson-card {
  background: #fff;
  padding: 15px;
  border-radius: 10px;
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.1);
}
.cart {
  margin-top: 30px;
  background: #fff;
  padding: 15px;
  border-radius: 10px;
}
.controls {
  display: flex;
  gap: 10px;
  margin-bottom: 15px;
}
</style>
