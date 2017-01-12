<template>
    <div class="add-todo__wrapper">
        <div v-on:click="hide" class="overlay"></div>
        <div class="add-todo">
            <input v-on:keyup.enter="add" v-model="text" class="add-todo__input" placeholder="Text...">
            <div class="buttons">
                <button v-on:click="hide" class="cancel__button button">Cancel</button>
                <button v-on:click="add" class="add-todo__button button">Add</button>
            </div>
        </div>
    </div>
</template>

<script type="text/javascript">
    export default {
        data: function() {
            return {
                text: '',
                active: false,
            }
        },
        methods: {
            add() {
                if ( this.text.length > 0 ) {
                    this.$emit('addTodo', this.text);
                }
                this.text = "";
            },
            hide() {
                this.$emit('hideAddTodo');
            }
        },
    }
</script>

<style src="./Add-todo.scss"></style>
