<template>
    <div>
        <b-form @submit="handleLogin">
            <b-row>
                <b-col sm="3"><label for="username">Username:</label></b-col>
                <b-col sm="9">
                    <b-form-input
                        id="username"
                        v-model="user.username"
                        type="text"
                        required
                        placeholder="UserName"
                    ></b-form-input>
                </b-col>
            </b-row>
            <b-row>
                <b-col sm="3"><label for="password">Password:</label></b-col>
                <b-col sm="9">
                    <b-form-input
                        id="password"
                        v-model="user.password"
                        type="password"
                        required
                        placeholder="Password"
                    ></b-form-input>
                </b-col>
            </b-row>
            <b-button type="submit" variant="primary">Submit</b-button>
            <b-button variant="danger" @click="registerUser">Register</b-button>
        </b-form>
        <b-card class="mt-3" header="Form Data Result">
            <pre class="m-0"></pre>
        </b-card>
    </div>
</template>

<script>
import User from '../models/user';

export default {
    name: 'Login',
    data() {
        return {
            user: new User('', '', ''),
            loading: false,
            message: ''
        }
    },
    computed: {
        loggedIn() {
            return this.$store.state.initialState.status.loggedIn;
        }
    },
    created() {
        if (this.loggedIn) {
            this.$router.push('/');
        }
    },
    methods: {
        handleLogin(evt) {
            evt.preventDefault();
            console.log(this.user.username);
            this.loading = true;
            this.$validator.validateAll().then(isValid=> {
                if(!isVaild) {
                    this.loading = false;
                    return;
                }

                if (this.user.username && this.user.password) {
                    this.$store.dispatch('login', this.user).then(
                        ()=> {
                            this.$router.push('/');
                        },
                        error => {
                            this.loading = false;
                            this.message = 
                            (error.response && error.response.data) || error.message || error.toString();
                        }
                    );
                }
            });
        }
    },

    registerUser() {
        this.$router.push({ name: 'Register'});
    }
    
}
</script>