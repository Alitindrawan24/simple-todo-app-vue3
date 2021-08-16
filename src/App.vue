<template>
    <div>
        <form id="form" @submit.prevent="addNewTodo">
            <input type="text" name="newTodo" v-model="newTodo">
            <button type="submit">Submit</button>
        </form>
        <button @click="markAsDoneAll">Mark as done all</button>
        <button @click="removeAll">Remove all</button>
        <button @click="removeAllDone">Remove all done todo</button>
        <ul>
            <li v-for="(todo, index) in todos" :key="todo.id">
                <h4 :class="{ done: todo.done }" @click="toggleTodo(todo)">{{ todo.name }}</h4>
                <button @click="removeTodo(index)">Remove</button>
            </li>
        </ul>
    </div>
</template>

<script>
import {ref} from 'vue';
export default {
    setup(){
        const newTodo = ref('');
        const todos = ref([]);

        function addNewTodo(){
            if(newTodo.value !== ''){
                todos.value.push({
                    name: newTodo.value,
                    id: Date.now(),
                    done: false,
                });
                newTodo.value = '';
            }
        }

        function removeTodo(index){
            todos.value.splice(index, 1);
        }

        function toggleTodo(todo){
            todo.done = !todo.done;
        }

        function markAsDoneAll(){
            todos.value.forEach(todo => todo.done = true);
        }

        function removeAll(){
            todos.value = [];
        }
        
        function removeAllDone(){
            todos.value.forEach((todo, index) => {
                if(todo.done){
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
        }
    }
};
</script>


<style>
.done{
    color: red;
    text-decoration: line-through;
}
</style>