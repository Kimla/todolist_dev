<template>
    <div class="todo-item__wrapper" v-bind:class="{ completed: todo.completed }">
        <div v-on:click="addFocus()" class="todo-item">
            <span class="text">{{ todo.text }}</span>
        </div>
        <div v-if="this.focus == true" class="todo-item todo-item__focus">
            <div class="overlay" v-on:click="removeFocus()"></div>
            <div class="todo-item__focus-inner">
                <input v-on:keyup="update" v-on:keyup.enter="removeFocus" autofocus v-model="todo.text">
                <span class="buttons">
                    <button class="button remove-button" v-on:click="$emit('remove')"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 40 40"><path d="M28 40H11.8c-3.3 0-5.9-2.7-5.9-5.9V16c0-.6.4-1 1-1s1 .4 1 1v18.1c0 2.2 1.8 3.9 3.9 3.9H28c2.2 0 3.9-1.8 3.9-3.9V16c0-.6.4-1 1-1s1 .4 1 1v18.1c0 3.2-2.7 5.9-5.9 5.9zm5.3-35.1h-7.6C25.2 2.1 22.8 0 19.9 0s-5.3 2.1-5.8 4.9H6.5C4.2 4.9 2.4 6.7 2.4 9s1.8 4 4.1 4h26.9c2.3 0 4.1-1.8 4.1-4.1s-1.9-4-4.2-4zM19.9 2c1.8 0 3.3 1.2 3.7 2.9h-7.5c.5-1.7 2-2.9 3.8-2.9zm13.4 9H6.5c-1.1 0-2.1-.9-2.1-2.1 0-1.1.9-2.1 2.1-2.1h26.9c1.1 0 2.1.9 2.1 2.1-.1 1.2-1 2.1-2.2 2.1z"/><path d="M12.9 35.1c-.6 0-1-.4-1-1V17.4c0-.6.4-1 1-1s1 .4 1 1v16.7c0 .5-.5 1-1 1zm14 0c-.6 0-1-.4-1-1V17.4c0-.6.4-1 1-1s1 .4 1 1v16.7c0 .5-.5 1-1 1zm-7 0c-.6 0-1-.4-1-1V17.4c0-.6.4-1 1-1s1 .4 1 1v16.7c0 .5-.5 1-1 1z"/></svg></button>
                    <button class="button complete-button" v-on:click="toggleComplete()"><svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24"><path fill-rule="evenodd" d="M21.652 3.21c-.293-.294-.77-.294-1.06 0L9.41 14.34c-.293.297-.77.297-1.062 0l-4.9-4.99c-.144-.147-.334-.22-.525-.22-.193-.002-.39.07-.536.22L.222 11.298c-.144.148-.222.333-.222.526 0 .194.078.397.223.544l4.94 5.184c.292.297.77.777 1.062 1.07l2.124 2.142c.29.293.767.293 1.06 0l14.366-14.34c.293-.294.293-.777 0-1.07l-2.125-2.14z" clip-rule="evenodd"/></svg></button>
                </span>
            </div>
        </div>
    </div>
</template>

<script>
    export default {
        props: ['todo', 'index'],
        data: function() {
            return {
                focus: false,
            }
        },
        methods: {
            addFocus() {
                this.focus = true;
                this.$emit('active');
            },
            removeFocus() {
                this.focus = false;
                this.$emit('inactive');
            },
            toggleComplete() {
                if ( this.todo.completed ) {
                    this.todo.completed = false;
                }
                else {
                    this.todo.completed = true;
                }
                this.update();
            },
            update() {
                this.$emit('update', this.todo);
            }
        },
    }
</script>

<style src="./todo-item.scss"></style>
