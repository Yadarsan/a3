<template>
  <div class="container my-5">
    <h2>News</h2>

    <div class="row g-2 mb-4">
      <div class="col-md-3">
        <input v-model="filters.title" class="form-control" placeholder="Search by title" />
      </div>
      <div class="col-md-3">
        <input v-model="filters.content" class="form-control" placeholder="Search by content" />
      </div>
      <div class="col-md-3">
        <input v-model="filters.category" class="form-control" placeholder="Search by category" />
      </div>
      <div class="col-md-3">
        <input v-model="filters.date" class="form-control" type="date" placeholder="Search by date" />
      </div>
    </div>

    <div v-for="item in filteredNews" :key="item.id" class="card my-2">
      <div class="card-body">
        <h5>{{ item.title }} ({{ item.category }})</h5>
        <p>{{ item.content }}</p>
        <small>{{ item.date }}</small>
      </div>
    </div>

    <div v-if="filteredNews.length === 0" class="alert alert-warning">
      No news matched your search criteria.
    </div>
  </div>
</template>

<script>
import news from '../data/news.json'

export default {
  data() {
    return {
      filters: {
        title: '',
        content: '',
        category: '',
        date: ''
      },
      newsList: news
    }
  },
  computed: {
    filteredNews() {
      return this.newsList.filter(item => {
        return (
          item.title.toLowerCase().includes(this.filters.title.toLowerCase()) &&
          item.content.toLowerCase().includes(this.filters.content.toLowerCase()) &&
          item.category.toLowerCase().includes(this.filters.category.toLowerCase()) &&
          item.date.includes(this.filters.date)
        )
      })
    }
  }
}
</script>
