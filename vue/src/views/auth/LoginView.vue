<template>
    <div id="login">
        <form v-on:submit.prevent="login">
            <h1>Please Sign In</h1>
            <div class="alert" role="alert" v-if="invalidCredentials">
                Invalid username and password!
            </div>
            <div role="alert" v-if="this.$route.query.registration">
                Thank you for registering, please sign in.
            </div>
            <div class="form-input-group">
                <label for="username">Username</label>
                <input type="text" id="username" v-model="user.username" required autofocus/>
            </div>
            <div class="form-input-group">
                <label for="password">Password</label>
                <input type="password" id="password" v-model="user.password" required/>
            </div>
            <button type="submit">Sign in</button>
<!--            <p>-->
<!--                <router-link v-bind:to="{ name: 'register' }">Need an account? Sign up.</router-link>-->
<!--            </p>-->
        </form>
    </div>
</template>

<script>
import authService from "../../services/AuthService";

export default {
    components: {},
    data() {
        return {
            user: {
                username: "",
                password: ""
            },
            invalidCredentials: false
        };
    },
    methods: {
        login() {
            authService
                    .login(this.user)
                    .then(response => {
                        if (response.status == 200) {
                            this.$store.commit("SET_AUTH_TOKEN", response.data.token);
                            this.$store.commit("SET_USER", response.data.user);
                            this.$router.push("/");
                        }
                    })
                    .catch(error => {
                        const response = error.response;

                        if (response.status === 401) {
                            this.invalidCredentials = true;
                        }
                    });
        }
    },
    created() {
      if (this.$store.state.token != '') {
        this.$router.push("/");
      }
    }

};
</script>

<style scoped>
#login {
    margin: auto;
    width: fit-content;
    padding: 1px 48px;
    background: #9dd9d2;
}

input {
    width: 100%;
}

.form-input-group {
    display: grid;
    grid-template-columns: 1fr 2fr;
    margin-bottom: 1rem;
}

label, h1 {
    margin-right: 0.5rem;
    color: #392f5a;
}

.alert {
    margin-bottom: 1rem;
    color: red;
}
</style>
