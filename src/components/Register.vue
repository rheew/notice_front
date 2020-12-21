<template>
    <div>
        <b-form @submit="handleRegister">
            <b-row>
                <b-col sm="3"><label for="username">Username:</label></b-col>
                <b-col sm="9">
                    <b-form-input
                        id="username"
                        v-model="user.username"
                        type="text"
                        required
                        placeholder="Username"
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
            <b-row>
                <b-col sm="3"><label for="email">E-mail:</label></b-col>
                <b-col sm="9">
                    <b-form-input
                        id="email"
                        v-model="user.email"
                        type="email"
                        required
                        placeholder="E-mail"
                    ></b-form-input>
                </b-col>
            </b-row>
            <b-button type="submit" variant="primary">Register</b-button>
        </b-form>
        <b-card class="mt-3" header="Form Data Result">
            <pre class="m-0"></pre>
        </b-card>
    </div>
</template>

<script>
import User from '../models/user';

export default {
    name: "Register",
    data() {
        return {
            user: new User('', '', ''),
            submitted: false,
            successful: false,
            message: '',
            confirm_password: '',
            isTermsConditionAccepted: true
        };

    },
    computed: {
        loggedIn() {
            return this.$store.state.initialState.status.loggedIn;
        }
    },
    mounted() {
        if (this.loggedIn) {
            this.$router.push('/');
        }
    },
    methods: {
        handleRegister(evt) {
            evt.preventDefault();
            this.message = '';
            this.submitted = true;
            this.$validator.validate().then(isValid => {
                if (idValid) {
                    this.$store.dispatch('register', this.user).then(
                        data => {
                            this.message = data.message;
                            this.successful - true;
                        },
                        error => {
                            this.message =
                                (error.response && error.response.data) ||
                                error.message ||
                                error.toString();
                            this.successful = false;
                        }
                    )
                }
            })
        }

    }
}
</script>