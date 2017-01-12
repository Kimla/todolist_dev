<template>
    <div class="login">
        <input type="text" placeholder="Email..." v-model="email" v-on:keyup.enter="login">
        <input type="password" placeholder="Password..." v-model="password" v-on:keyup.enter="login">
        <button class="login-button" v-on:click="login">Login</button>
        <button class="signup-button" v-on:click="signup">Signup</button>
    </div>
</template>

<script type="text/javascript">
    export default {
        data: function() {
            return {
                email: '',
                password: '',
            }
        },
        methods: {
            login() {
                this.$emit('login', this.email, this.password );
            },
            signup() {
                this.$emit('signup', this.email, this.password );
            },
        },
    }
</script>

<style src="./Login.scss"></style>
