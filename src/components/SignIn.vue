<template lang="pug">
    div
        form.mt-5(@submit.prevent="enterUser", v-if="show")
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
        .alert.alert-success.mt-5(role="alert", v-if="signSuccess")
            strong Поздравляю!
                |  Вы вошли в систему.
        .alert.alert-danger.mt-5(role="alert", v-if="signError")
            strong Упс!
                |  Что-то пошло не так.
</template>

<script>
    export default {
        data() {
            return {
                show: true,
                signSuccess: false,
                signError: false,
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
                        const sett = {
                            uid: response.uid,
                            email: response.email,
                            mainPage: true,
                            signComplete: true
                        };
                        this.$emit('addUser', sett);
                        this.show = false;
                        this.signSuccess = true;
                    })
                    .catch( error => {
                        console.log(error);
                    })
            }
        }
    }
</script>