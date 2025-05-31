<template>
  <div class="container my-5">
    <h2>Property Listings</h2>

    <!-- Advanced Search -->
    <div class="card p-3 mb-4 bg-light border">
      <div class="row g-2">
        <div class="col-md-4">
          <input v-model="searchQuery" class="form-control" placeholder="Search by title or location" />
        </div>
        <div class="col-md-2">
          <select v-model="selectedType" class="form-select">
            <option value="">All Types</option>
            <option>House</option>
            <option>Apartment</option>
            <option>Villa</option>
            <option>Bungalow</option>
          </select>
        </div>
        <div class="col-md-2">
          <select v-model="selectedBedrooms" class="form-select">
            <option value="">Any Bedrooms</option>
            <option v-for="n in 5" :key="n" :value="n">{{ n }}+</option>
          </select>
        </div>
        <div class="col-md-2">
          <input type="number" v-model.number="minPrice" class="form-control" placeholder="Min Price" />
        </div>
        <div class="col-md-2">
          <input type="number" v-model.number="maxPrice" class="form-control" placeholder="Max Price" />
        </div>
      </div>
    </div>

    <!-- Listings -->
    <div class="row">
      <div v-for="p in filteredProperties" :key="p.id" class="col-md-4 mb-4">
        <!-- Edit Form -->
        <div v-if="editingId === p.id" class="card p-3">
          <h5>Edit Listing</h5>
          <form @submit.prevent="saveEdit" class="row g-2">
            <div class="col-12">
              <input v-model="editedListing.title" required class="form-control" placeholder="Title" />
            </div>
            <div class="col-12">
              <input v-model="editedListing.description" required class="form-control" placeholder="Description" />
            </div>
            <div class="col-6">
              <input v-model="editedListing.location" required class="form-control" placeholder="Location" />
            </div>
            <div class="col-3">
              <select v-model="editedListing.type" required class="form-select">
                <option disabled value="">Type</option>
                <option>House</option>
                <option>Apartment</option>
                <option>Villa</option>
                <option>Bungalow</option>
              </select>
            </div>
            <div class="col-3">
              <input type="number" v-model.number="editedListing.bedrooms" required class="form-control" placeholder="Bedrooms" min="1" />
            </div>
            <div class="col-6">
              <input type="number" v-model.number="editedListing.price" required class="form-control" placeholder="Price" min="0" />
            </div>
            <div class="col-6 d-flex justify-content-end gap-2">
              <button type="submit" class="btn btn-success">Save</button>
              <button type="button" class="btn btn-secondary" @click="cancelEdit">Cancel</button>
            </div>
          </form>
        </div>

        <!-- Property Card with Edit/Delete buttons -->
        <div v-else class="card p-3 h-100">
          <h5>{{ p.title }}</h5>
          <p>{{ p.description }}</p>
          <p class="text-muted">{{ p.location }}</p>
          <p><strong>Type:</strong> {{ p.type }}</p>
          <p><strong>Bedrooms:</strong> {{ p.bedrooms }}</p>
          <p class="fw-bold">${{ p.price.toLocaleString() }}</p>
          <div class="d-flex justify-content-between">
            <button @click="editListing(p.id)" class="btn btn-sm btn-primary">Edit</button>
            <button @click="deleteListing(p.id)" class="btn btn-sm btn-danger">Delete</button>
          </div>
        </div>
      </div>
    </div>

    <div v-if="filteredProperties.length === 0" class="alert alert-warning mt-4">
      No properties found matching your criteria.
    </div>

    <!-- Add New Listing -->
    <div class="card p-3 mt-4">
      <h5>Add New Listing</h5>
      <form @submit.prevent="addListing" class="row g-2">
        <div class="col-md-4">
          <input v-model="newListing.title" required class="form-control" placeholder="Title" />
        </div>
        <div class="col-md-4">
          <input v-model="newListing.description" required class="form-control" placeholder="Description" />
        </div>
        <div class="col-md-2">
          <input v-model="newListing.location" required class="form-control" placeholder="Location" />
        </div>
        <div class="col-md-2">
          <select v-model="newListing.type" required class="form-select">
            <option disabled value="">Type</option>
            <option>House</option>
            <option>Apartment</option>
            <option>Villa</option>
            <option>Bungalow</option>
          </select>
        </div>
        <div class="col-md-2">
          <input type="number" v-model.number="newListing.bedrooms" required class="form-control" placeholder="Bedrooms" min="1" />
        </div>
        <div class="col-md-2">
          <input type="number" v-model.number="newListing.price" required class="form-control" placeholder="Price" min="0" />
        </div>
        <div class="col-md-12 d-flex justify-content-end mt-2">
          <button type="submit" class="btn btn-success">Add Listing</button>
        </div>
      </form>
    </div>

  </div>
</template>

<script>
import properties from '../data/properties.json'

export default {
  data() {
    return {
      searchQuery: '',
      selectedType: '',
      selectedBedrooms: '',
      minPrice: '',
      maxPrice: '',
      list: [...properties],

      editingId: null,
      editedListing: null,

      newListing: {
        title: '',
        description: '',
        location: '',
        type: '',
        bedrooms: null,
        price: null,
      }
    }
  },
  computed: {
    filteredProperties() {
      return this.list.filter(p => {
        const matchesQuery =
          p.title.toLowerCase().includes(this.searchQuery.toLowerCase()) ||
          p.location.toLowerCase().includes(this.searchQuery.toLowerCase())

        const matchesType = this.selectedType ? p.type === this.selectedType : true
        const matchesBedrooms = this.selectedBedrooms ? p.bedrooms >= this.selectedBedrooms : true
        const matchesMinPrice = this.minPrice ? p.price >= this.minPrice : true
        const matchesMaxPrice = this.maxPrice ? p.price <= this.maxPrice : true

        return matchesQuery && matchesType && matchesBedrooms && matchesMinPrice && matchesMaxPrice
      })
    }
  },
  methods: {
    editListing(id) {
      const listing = this.list.find(p => p.id === id)
      if (listing) {
        this.editingId = id
        this.editedListing = { ...listing } // clone object for editing
      }
    },
    saveEdit() {
      const index = this.list.findIndex(p => p.id === this.editingId)
      if (index !== -1) {
        this.list.splice(index, 1, { ...this.editedListing })
        this.cancelEdit()
      }
    },
    cancelEdit() {
      this.editingId = null
      this.editedListing = null
    },
    deleteListing(id) {
      this.list = this.list.filter(p => p.id !== id)
      // cancel editing if the edited item got deleted
      if (this.editingId === id) {
        this.cancelEdit()
      }
    },
    addListing() {
      // Simple validation (you can expand)
      if (!this.newListing.title || !this.newListing.description || !this.newListing.location || !this.newListing.type || !this.newListing.bedrooms || !this.newListing.price) {
        alert("Please fill all fields")
        return
      }
      // Generate a new unique ID (simple approach)
      const newId = this.list.length ? Math.max(...this.list.map(p => p.id)) + 1 : 1

      this.list.push({
        id: newId,
        title: this.newListing.title,
        description: this.newListing.description,
        location: this.newListing.location,
        type: this.newListing.type,
        bedrooms: this.newListing.bedrooms,
        price: this.newListing.price
      })

      // Reset the new listing form
      this.newListing = {
        title: '',
        description: '',
        location: '',
        type: '',
        bedrooms: null,
        price: null,
      }
    }
  }
}
</script>
