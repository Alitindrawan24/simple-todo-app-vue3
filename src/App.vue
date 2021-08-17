<template>
  <div>
    <form id="form" @submit.prevent="addNewTodo">
      <input type="text" name="newTodo" v-model="newTodo" />
      <button type="submit">Submit</button>
    </form>
    <button @click="markAsDoneAll">Mark as done all</button>
    <button @click="removeAll">Remove all</button>
    <button @click="removeAllDone">Remove all done todo</button>
    <draggable v-model="todos" tag="transition-group" item-key="id">
      <template #item="{ element }">
          <li class="list-group-item">
            <i
              @click="element.done = !element.done"
              aria-hidden="true"
            ></i>
            {{ element.name }}
          </li>
        </template>
    </draggable>
    <rawDisplayer class="col-3" :value="todos" title="List" />
  </div>
</template>

<script>
import { ref } from "vue";
import draggable from "vuedraggable";

export default {
  components: {
    draggable,
  },
  computed: {
      dragOptions() {
      return {
        animation: 200,
        group: "description",
        disabled: false,
        ghostClass: "ghost"
      };
    }
  },
  setup() {
    const newTodo = ref("");
    const todos = ref([]);

    function addNewTodo() {
      if (newTodo.value !== "") {
        todos.value.push({
          name: newTodo.value,
          id: Date.now(),
          done: false,
        });
        newTodo.value = "";
      }
    }

    function removeTodo(index) {
      todos.value.splice(index, 1);
    }

    function toggleTodo(todo) {
      todo.done = !todo.done;
    }

    function markAsDoneAll() {
      todos.value.forEach((todo) => (todo.done = true));
    }

    function removeAll() {
      todos.value = [];
    }

    function removeAllDone() {
      todos.value.forEach((todo, index) => {
        if (todo.done) {
          removeTodo(index);
        }
      });
    }

    return {
      newTodo,
      todos,
      addNewTodo,
      removeTodo,
      toggleTodo,
      markAsDoneAll,
      removeAll,
      removeAllDone,
    };
  },
};
</script>


<style>
.done {
  color: red;
  text-decoration: line-through;
}
.button {
  margin-top: 35px;
}
.flip-list-move {
  transition: transform 0.5s;
}
.no-move {
  transition: transform 0s;
}
.ghost {
  opacity: 0.5;
  background: #c8ebfb;
}
.list-group {
  min-height: 20px;
}
.list-group-item {
  cursor: move;
}
.list-group-item i {
  cursor: pointer;
}
</style>