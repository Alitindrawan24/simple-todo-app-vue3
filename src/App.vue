<template>
    <div>
        <form id="form" @submit.prevent="addNewTodo">
            <input type="text" name="newTodo" v-model="newTodo" />
            <button type="submit">Submit</button>
        </form>
        <button @click="markAsDoneAll">Mark as done all</button>
        <button @click="removeAll">Remove all</button>
        <button @click="removeAllDone">Remove all done todo</button>
        <div class="col-6">
            <draggable
                class="list-group"
                v-model="todos"
                :component-data="{
                tag: 'ul',
                type: 'transition-group',
                name: !drag ? 'flip-list' : null
            }"
                v-bind="dragOptions"
                @start="drag = true"
                @end="drag = false"
                item-key="order"
            >
                <template #item="{ element }">
                    <li class="list-group-item">
                        <i @click="element.done = !element.done" aria-hidden="true"></i>
                        {{ element.name }}
                    </li>
                </template>
            </draggable>
        </div>
    </div>
</template>

<script>
import { ref } from "vue";
import draggable from "vuedraggable";
import "bootstrap/dist/css/bootstrap.min.css";

export default {
    components: {
        draggable,
    },
    data() {
        return {
            drag: false
        };
    },
    computed: {
        dragOptions() {
            return {
                animation: 200,
                group: "description",
                disabled: false,
                ghostClass: "ghost",
            };
        },
    },
    setup() {
        const newTodo = ref("");
        const todos = ref([
            {
                'id' : 1,
                'name' : 'Todo 1',
                'done' : false
            },
            {
                'id' : 2,
                'name' : 'Todo 2',
                'done' : false
            },
            {
                'id' : 3,
                'name' : 'Todo 3',
                'done' : false
            },
        ]);

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
body {
    background-color: #e9ecef;
}
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