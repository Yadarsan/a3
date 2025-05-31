<template>
  <div class="container my-5">
    <h2>About This App</h2>
    <p>This app is a real estate listing platform built with Vue 3 and Bootstrap.</p>

    <input v-model="firstName" class="form-control my-2" placeholder="First Name" />
    <input v-model="lastName" class="form-control mb-2" placeholder="Last Name" />

    <p class="alert alert-success">Welcome, {{ fullName }}</p>

    <div class="form-check">
      <input
        class="form-check-input"
        type="radio"
        id="mountain"
        value="/images/mountain.jpg"
        v-model="selectedImage"
      />
      <label class="form-check-label" for="mountain">Mountain</label>
    </div>

    <div class="form-check">
      <input
        class="form-check-input"
        type="radio"
        id="ocean"
        value="/images/ocean.jpg"
        v-model="selectedImage"
      />
      <label class="form-check-label" for="ocean">Ocean</label>
    </div>

    <img
      v-if="selectedImage"
      :src="selectedImage"
      class="img-fluid mt-3"
      alt="Selected Image"
    />
  </div>
</template>

<script>
export default {
  data() {
    return {
      firstName: '',
      lastName: '',
      selectedImage: ''
    }
  },
  computed: {
    fullName() {
      return `${this.firstName} ${this.lastName}`.trim()
    }
  }
}
</script>
