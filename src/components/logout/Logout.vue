<template>
    <div class="logout">
        <p class="logged-in-as">Logged in as: {{ currentUser }}</p>
        <button class="logout-button" v-on:click="logout">logout</button>
    </div>
</template>

<script type="text/javascript">
    export default {
        props: ['currentUser'],
        methods: {
            logout() {
                this.$emit('logout');
            },
        },
    }
</script>

<style src="./Logout.scss"></style>
