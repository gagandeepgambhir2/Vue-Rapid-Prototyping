<template>
  <h2>To do: ({{ todos.length }})</h2>
  <input type="text" v-model="todo" v-on:keyup.enter="handleSaveTodo" placeholder="Type something and press ENTER" />

  <ol>
    <draggable :list="todos" :item-key="element" class="list-group" ghost-class="ghost" @start="true"
      @end="dragging = false">

      <template #item="{ element }">
        <li>
          <input type="checkbox" :key="element" :name="element" @click="toggleTaskCompleted(element, 'pending')"
            :value="element" />&nbsp;
          <label :for="element">{{ element }}</label>
          &nbsp;
          <a href="javascript:;" @click="handleRemove(element, 'pending')" class="remove">Remove</a>
        </li>
      </template>
    </draggable>
  </ol>
  <hr />
  <h2>
    Completed items: ({{ completed.length }})
  </h2>
  <ol>
    <draggable :list="completed" :item-key="element" class="list-group" ghost-class="ghost" @start="true"
      @end="dragging = false">

      <template #item="{ element }">
        <li>
          <input type="checkbox" :checked="checked" :key="element" :name="element"
            @click="toggleTaskCompleted(element, 'completed')" v-model="selected" value="checked" />&nbsp;
          <label :for="element">{{ element }}</label>
          &nbsp;
          <a href="javascript:;" @click="handleRemove(element, 'completed')" class="remove">Remove</a>
        </li>
      </template>
    </draggable>
  </ol>
</template>

<script>
import draggable from 'vuedraggable'
export default {
  name: 'App',
  components: {
    draggable
  },
  data: function () {
    return {
      todos: [],
      completed: [],
      todo: "",
      selected: true
    }

  },

  methods: {
    // Handles save todo
    handleSaveTodo() {
      console.log(this.todo, "this.todothis.todo")
      this.todos.push(this.todo)
      this.todo = "";
    },
    // handles toggle task completed
    toggleTaskCompleted(val, type) {
      let data = type == 'pending' ? this.todos : this.completed;
      let filtered = data.filter(v => v != val);
      if (type == 'pending') {

        this.todos = filtered;
        this.completed.push(val);
      }
      else {
        this.completed = filtered;
        this.todos.push(val);
      }
    },
    // handles remove
    handleRemove(val, type) {
      let data = type == 'pending' ? this.todos : this.completed;
      let filtered = data.filter(v => v != val);
      data = filtered;
      if (type == 'pending') {
        this.todos = filtered;
      } else {
        this.completed = filtered;
      }
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

li {
  padding: 10px;
  border: 1px solid;
  margin: 10px;
  list-style: none;
  text-align: center;
  cursor: pointer;
}

.draggable-list {
  background: #3f51b5;
  border: 1px solid;
  height: 50vh;
}
</style>
