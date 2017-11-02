<template lang="pug">
    form.mt-5(@submit.prevent="enterUser")
        .form-group
            label(for="email") Ваш email
            input.form-control#email(type="email", placeholder="Введите email:", required, v-model="user.email")
        .form-group
            label(for="password") Ваш пароль
            input.form-control#password(type="password", placeholder="Введите пароль:", required, v-model="user.password", minlength="6")
        //.alert.alert-danger(role="alert", v-if="error")
            strong Упс!
                |  Пароли не совпадают или вы забыли их ввести.
        button.btn.btn-primary(type="submit") Войти
</template>

<script>
    export default {
        data() {
            return {
                user: {
                    email: '',
                    password: '',
                }
            }
        },
        methods: {
            enterUser() {
                firebase.auth().signInWithEmailAndPassword(this.user.email, this.user.password)
                    .then( ( response ) => {
                        //this.$emit('regSuccess', 'sign-in');
                        console.log(response);
                        const sett = {
                            uid: response.uid,
                            email: response.email,
                            mainPage: true,
                            complete: true
                        }
                        this.$emit('addUser', sett)
                    })
                    .catch( error => {
                        console.log(error);
                    })
            }
        }
    }
</script>