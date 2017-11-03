<template lang="pug">
    div
        form.mt-5(@submit.prevent="registerUser", v-if="show")
            .form-group
                label(for="email") Ваш email
                input.form-control#email(type="email", placeholder="Введите email:", required, v-model="user.email")
            .form-group
                label(for="password") Ваш пароль (минимум 6 символов)
                input.form-control#password(type="password", placeholder="Введите пароль:", required, v-model="user.password", minlength="6")
            .form-group
                label(for="password2") Повторите пароль
                input.form-control#password2(type="password", placeholder="Повторите пароль:", required, v-model="user.confirmPassword", minlength="6")
            .alert.alert-danger.mt-5(role="alert", v-if="errorConfirm")
                strong Упс!
                    |  Пароли не совпадают
            .alert.alert-danger.mt-5(role="alert", v-if="errorSmall")
                strong Упс!
                    |  Пароль должен быть длиной не менее 6 символов
            button.btn.btn-primary(type="submit") Зарегистрироваться

        .alert.alert-success.mt-5(role="alert", v-if="signSuccess")
            strong Поздравляю!
                |  Вы вошли в систему.
        .alert.alert-danger.mt-5(role="alert", v-if="signError")
            strong Упс!
                |  Что-то пошло не так.
</template>

<script>
    export default {
        name: 'sign-up',
        data() {
            return {
                show: true,
                signSuccess: false,
                signError: false,
                user: {
                    email: '',
                    password: '',
                    confirmPassword: ''
                },
                errorConfirm: false,
                errorSmall: false
            }
        },
        methods: {
            registerUser() {
                this.errorConfirm = false;
                this.errorSmall = false;
                if (this.user.password !== this.user.confirmPassword) {
                    this.errorConfirm = true;
                } else if (this.user.password.length < 6) {
                    this.errorSmall = true;
                } else {
                    firebase.auth().createUserWithEmailAndPassword(this.user.email, this.user.password)
                        .then(() => {
                            this.$emit('regSuccess', 'sign-in');
                            this.show = false;
                            this.signSuccess = true;
                        })
                        .catch(error => {
                            console.log(error);
                        })
                }
            }
        }
    }
</script>
