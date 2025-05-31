<template>
  <div class="container my-5" style="max-width: 400px;">
    <h2>Login</h2>
    <form @submit.prevent="loginUser" novalidate>
      <input
        class="form-control my-2"
        v-model.trim="email"
        type="email"
        required
        placeholder="Email"
      />
      <input
        class="form-control my-2"
        v-model="password"
        type="password"
        required
        placeholder="Password"
      />
      <button class="btn btn-success" :disabled="!email || !password">Login</button>
    </form>

    <div v-if="errorMessage" class="text-danger mt-3">{{ errorMessage }}</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      email: '',
      password: '',
      errorMessage: ''
    }
  },
  methods: {
    loginUser() {
      const users = JSON.parse(localStorage.getItem('users') || '[]')
      const user = users.find(u => u.email === this.email && u.password === this.password)

      if (user) {
        this.errorMessage = ''
        alert(`Logged in successfully as ${this.email}`)
        // You can redirect or do other stuff here
      } else {
        this.errorMessage = 'Invalid email or password'
      }
    }
  }
}
</script>
