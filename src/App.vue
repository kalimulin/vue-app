<template lang="pug">
    div
        .container-fluid.navbar.navbar-dark.bg-dark
            .navbar-nav.ml-auto
                .form-inline.d-flex.justify-content-end
                    div(v-if="!signComplete")
                        button.btn.btn-outline-success.mr-3(type="button", @click="switchSign('sign-in')") Войти
                        button.btn.btn-outline-success(type="button", @click="switchSign('sign-up')") Регистрация
                    span(v-else) {{ email }}
        .container(v-if = "!isMainPage")
            .row
                .col
                    sign-in(v-if="sign === 'sign-in'",
                        @addUser="isMainPage = $event.mainPage, signComplete = $event.complete, email = $event.email, uid = $event.uid")
                    sign-up(v-else, @regSuccess="sign = $event")
        .container(v-else)
            .row
                .col
                    main-page(:uid="uid")

</template>

<script>
    import SignIn from './components/SignIn.vue'
    import SignUp from './components/SignUp.vue'
    import MainPage from './components/MainPage.vue'

    export default {
        name: 'app',
        data() {
            return {
                sign: 'sign-in',
                isMainPage: false,
                signComplete: false,
                email: '',
                uid: ''
            }
        },
        components: {
            SignIn,
            SignUp,
            MainPage

        },
        methods: {
            switchSign(currentSign) {
                this.sign = currentSign;
            }
        }
    }
</script>

<style lang="scss">
    span {
        color: #fff;
    }
</style>
