<template>
  <div class="container my-5" style="max-width: 400px;">
    <h2>Register</h2>
    <form @submit.prevent="registerUser" novalidate>
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
        @input="validatePassword"
      />
      
      <ul class="text-danger" v-if="passwordTouched">
        <li :class="{'text-success': hasMinLength}">At least 8 characters</li>
        <li :class="{'text-success': hasUpperCase}">Contains uppercase letter</li>
        <li :class="{'text-success': hasLowerCase}">Contains lowercase letter</li>
        <li :class="{'text-success': hasNumber}">Contains a number</li>
        <li :class="{'text-success': hasSpecialChar}">Contains special character (!@#$%^&*)</li>
      </ul>

      <button class="btn btn-primary mt-2" :disabled="!canRegister">Register</button>
    </form>

    <div v-if="message" :class="messageClass" class="mt-3">{{ message }}</div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      email: '',
      password: '',
      passwordTouched: false,
      hasMinLength: false,
      hasUpperCase: false,
      hasLowerCase: false,
      hasNumber: false,
      hasSpecialChar: false,
      message: '',
      messageClass: ''
    }
  },
  computed: {
    canRegister() {
      return (
        this.email &&
        this.hasMinLength &&
        this.hasUpperCase &&
        this.hasLowerCase &&
        this.hasNumber &&
        this.hasSpecialChar
      )
    }
  },
  methods: {
    validatePassword() {
      this.passwordTouched = true
      this.hasMinLength = this.password.length >= 8
      this.hasUpperCase = /[A-Z]/.test(this.password)
      this.hasLowerCase = /[a-z]/.test(this.password)
      this.hasNumber = /\d/.test(this.password)
      this.hasSpecialChar = /[!@#$%^&*]/.test(this.password)
    },
    registerUser() {
      if (!this.canRegister) {
        this.message = "Password does not meet requirements."
        this.messageClass = "text-danger"
        return
      }

      const users = JSON.parse(localStorage.getItem('users') || '[]')
      if (users.find(u => u.email === this.email)) {
        this.message = "Email is already registered."
        this.messageClass = "text-danger"
        return
      }

      users.push({ email: this.email, password: this.password })
      localStorage.setItem('users', JSON.stringify(users))

      this.message = "Registration successful! You can now login."
      this.messageClass = "text-success"
      
      this.email = ''
      this.password = ''
      this.passwordTouched = false
    }
  }
}
</script>
