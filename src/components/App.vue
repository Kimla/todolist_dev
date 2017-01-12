<template>
    <div id="app" v-if="this.loaded == true">
        <todolist v-if="this.loggedIn == true" :todolistRef="todolistRef" :todos="todos"></todolist>
        <logout v-if="this.loggedIn == true" @logout="logout" :currentUser="currentUser"></logout>
        <login v-if="this.loggedIn == false" @login="login" @signup="signup"></login>
    </div>
</template>

<script>
    import Todolist from './todolist/Todolist.vue';
    import Login from './login/Login.vue';
    import Logout from './logout/Logout.vue';
    import Vue from 'vue'

    var VueFire = require('vuefire')
    var Firebase = require('firebase')
    Vue.use(VueFire)

    var config = {
        apiKey: "AIzaSyCnbmcx_h0MlOSqlsRzrqf9Snkhg1TeNgo",
        authDomain: "todolist-426a4.firebaseapp.com",
        databaseURL: "https://todolist-426a4.firebaseio.com",
        storageBucket: "todolist-426a4.appspot.com",
        messagingSenderId: "802197748637"
    };

    var firebaseApp = Firebase.initializeApp(config);
    var db = firebaseApp.database();

    export default {
        components: {
            'todolist': Todolist,
            'login': Login,
            'logout': Logout
        },
        data: function() {
            return {
                todolistRef: [],
                todos: [],
                loggedIn: false,
                currentUser: false,
                userId: false,
                loaded: false
            }
        },
        methods: {
            login(email, password) {
                const auth = firebaseApp.auth();
                const promise = auth.signInWithEmailAndPassword(email, password);

                promise.catch(e => console.log(e.message));
            },
            logout() {
                firebaseApp.auth().signOut();
            },
            signup(email, password) {
                const auth = firebaseApp.auth();
                const promise = auth.createUserWithEmailAndPassword(email, password);

                promise.catch(e => console.log(e.message));
            },
        },
        created: function() {
            firebaseApp.auth().onAuthStateChanged(firebaseUser => {
                if ( firebaseUser ) {
                    this.loggedIn = true;
                    this.currentUser = firebaseUser.email;
                    this.userId = firebaseUser.uid;

                    this.todolistRef = db.ref(this.userId);
                    this.$bindAsArray('todos', db.ref(this.userId).orderByChild("sortOrder"));
                }
                else {
                    this.loggedIn = false;
                    this.currentUser = false;
                    this.userId = false;
                }
                this.loaded = true;
            });
        }
    }
</script>

<style src="./App.scss"></style>
