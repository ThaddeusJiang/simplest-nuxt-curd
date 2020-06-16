<template>
  <div class="whole">
    <h1>TODO Detail</h1>
    <form @submit.prevent="update">
      <input v-model="todo.content" type="text" />

      <button type="submit">Update</button>
    </form>
    <button class="danger" @click="remove">Delete</button>
  </div>
</template>

<script>
export default {
  async fetch() {
    this.todo = await this.$axios.$get(`/api/todos/${this.$route.params.id}`)
  },
  data() {
    return {
      todo: {
        content: ''
      }
    }
  },
  methods: {
    async update() {
      await this.$axios.$put(`/api/todos/${this.$route.params.id}`, this.todo)
      this.$router.push('/todos')
    },
    async remove() {
      await this.$axios.$delete(`/api/todos/${this.$route.params.id}`)
      this.$router.push('/todos')
    }
  }
}
</script>

<style scoped>
.whole {
  margin: 20vh auto;
  max-width: 800px;
}

form {
  max-width: 800px;
  display: flex;
  flex-direction: column;
}

form > * {
  margin-top: 16px;
}

.danger {
  background-color: red;
  width: 100%;
}
</style>
