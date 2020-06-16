<template>
  <div class="whole">
    <section class="list">
      <h1>TODOs</h1>

      <p v-if="$fetchState.pending">Fetching todos...</p>
      <p v-else-if="$fetchState.error">
        Error while fetching todos: {{ $fetchState.error.message }}
      </p>
      <ul v-else>
        <li v-for="todo of todos" :key="todo.id">
          <n-link :to="`/todos/${todo.id}`">{{ todo.content }}</n-link>
        </li>
      </ul>
    </section>

    <section>
      <form id="todo-form" @submit.prevent="add">
        <input
          v-model="input.content"
          type="text"
          placeholder="input your todo"
        />
        <button type="submit" :disabled="_valid">add</button>
      </form>
    </section>
  </div>
</template>

<script>
export default {
  async fetch() {
    this.todos = await this.$axios.$get('api/todos')
  },
  data() {
    return {
      todos: [],
      input: {
        content: ''
      }
    }
  },
  computed: {
    _valid() {
      return this.input.content === ''
    }
  },

  methods: {
    async add() {
      await this.$axios.$post('api/todos', this.input)
      this.input = {
        content: ''
      }
      this.todos = await this.$axios.$get('api/todos')
    }
  }
}
</script>

<style scoped>
.whole {
  margin-top: 20vh;
  text-align: center;
}

#todo-form {
  margin: 40px auto;
  max-width: 300px;
  display: flex;
  flex-direction: column;
}

#todo-form > * {
  margin-top: 16px;
}
</style>
