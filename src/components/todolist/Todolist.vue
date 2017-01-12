<template id="todolist">
    <div class="todolist-wrapper">
        <div class="todolist__header">
            <h1>Todo</h1>
            <button v-on:click="showAddTodo()" class="show-add-todo-button"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 256 256"><path d="M185.066 128c0 3.534-2.866 6-6.4 6H134v44.666c0 3.534-2.467 6.4-6 6.4-3.534 0-6-2.866-6-6.4V134H77.332c-3.535 0-6.4-2.466-6.4-6s2.866-6 6.4-6H122V77.33c0-3.533 2.466-6.4 6-6.4 3.533 0 6 2.867 6 6.4V122h44.666c3.534 0 6.4 2.466 6.4 6zM256 128C256 57.42 198.58 0 128 0S0 57.42 0 128s57.42 128 128 128 128-57.42 128-128zm-12.8 0c0 63.52-51.68 115.2-115.2 115.2-63.522 0-115.2-51.68-115.2-115.2C12.8 64.478 64.478 12.8 128 12.8c63.52 0 115.2 51.678 115.2 115.2z"/></svg></button>
        </div>
        <ul class="todolist">
            <draggable :list="todos" @end="updateSortOrder()" :options="{'disabled':draggingDisabled}">
                <li v-for="(todo, key) in todos" v-bind:key="todo.sortOrder">
                    <todo-item :todo="todo" :index="key" @update="updateTodo"  @inactive="enableDragging()" @active="disableDragging()" @remove="removeTodo(todo)"></todo-item>
                </li>
            </draggable>
        </ul>
        <add-todo v-if="this.showAddInput == true" @hideAddTodo="hideAddTodo" @addTodo="addTodo"></add-todo>
    </div>
</template>

<script>
    import TodoItem from '../todo-item/Todo-item.vue';
    import Draggable from 'vuedraggable';
    import AddTodo from '../add-todo/add-todo.vue';

    export default {
        components: {
            'todo-item': TodoItem,
            'draggable': Draggable,
            'add-todo': AddTodo,
        },
        props: ['todos', 'todolistRef'],
        data: function() {
            return {
                draggingDisabled: false,
                showAddInput: false,
            }
        },
        beforeMount: function() {
            this.todos.sort(function(a, b) {
                return a.sortOrder - b.sortOrder;
            });
        },
        methods: {
            addTodo(text) {
                this.todolistRef.push({
                    text: text,
                    completed: false,
                    sortOrder: -1
                });
                this.showAddInput = false;
                this.enableDragging();
            },
            disableDragging() {
                this.draggingDisabled = true;
            },
            enableDragging() {
                this.draggingDisabled = false;
            },
            hideAddTodo() {
                this.showAddInput = false;
                this.enableDragging();
            },
            showAddTodo() {
                this.showAddInput = true;
                this.disableDragging();
            },
            removeTodo(todo) {
                this.todolistRef.child(todo['.key']).remove();
            },
            updateTodo(todo) {
                this.todolistRef.child(todo['.key']).update({completed: todo.completed, text: todo.text});
            },
            updateSortOrder() {
                var keys = [];

                for (var i = 0; i < this.todos.length; i++) {
                    keys.push(this.todos[i][".key"]);
                }

                for (var i = 0; i < keys.length; i++) {
                    this.todolistRef.child(keys[i]).update({sortOrder: i});
                }
            },
        },
    }

</script>

<style src="./Todolist.scss"></style>
