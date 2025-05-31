<template>
  <div class="col-md-4 mb-4">
    <div class="card h-100">
      <div class="card-body">
        <h5 class="card-title">{{ property.title }}</h5>
        <p class="card-text">{{ property.description }}</p>
        <ul class="list-unstyled small">
          <li><strong>Location:</strong> {{ property.location }}</li>
          <li><strong>Type:</strong> {{ property.type }}</li>
          <li><strong>Bedrooms:</strong> {{ property.bedrooms }}</li>
          <li><strong>Price:</strong> ${{ property.price.toLocaleString() }}</li>
        </ul>
      </div>
      <div class="card-footer d-flex justify-content-between">
        <button class="btn btn-sm btn-warning" @click="$emit('edit')">Edit</button>
        <button class="btn btn-sm btn-danger" @click="$emit('delete')">Delete</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: ['property']
}
</script>
