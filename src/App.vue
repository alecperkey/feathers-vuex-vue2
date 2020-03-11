<template>
  <div class="root">
    <form @submit.prevent="create({ name: name.pop() })">
      <div class="card">
        <div class="card-content">
          <div class="field">
            <input v-model="name[0]" />
            <label>Create New To-do: {{ name[0] }}</label>
          </div>
        </div>
        <div class="action-bar">
          <button type="submit" :disabled="!name[0]" class="primary">
            Create
          </button>
        </div>
      </div>
    </form>

    <content class="list">
      <form
        v-for="todo in todos().data"
        :key="todo.id"
        @submit.prevent="remove(todo.id)"
      >
        <div class="card">
          <div class="card-header">
            <div class="title">{{ todo.name }}</div>
          </div>
          <div class="action-bar">
            <button class="primary" type="submit">Done</button>
          </div>
        </div>
      </form>
    </content>
  </div>
</template>

<script>
import { mapActions, mapGetters } from 'vuex'
export default {
  data () {
    return { name: [''] }
  },
  computed: {
    // only getters have live queries
    ...mapGetters('todos', { todos: 'find' })
  },
  methods: {
    ...mapActions('todos', ['create', 'remove', 'find'])
  },
  created: async function () {
    await this.find()
  }
}
</script>

<style lang="scss" scoped>
.root > form {
  padding: 15px;
  background-color: '#2244aa';
  border: 'solid 2px blue';
}
.root > form > .card {
  margin: auto;
  width: 320px;
}
.list > form {
  display: inline-block;
  margin: 8px;
  margin-right: 0;
}
.list > form > .card {
  width: 320px;
}
</style>
