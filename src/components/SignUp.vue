<template lang="pug">
  form.mt-5(@submit.prevent="registerUser")
    .form-group
      label(for="email") Ваш email
      input.form-control#email(type="email", placeholder="Введите email:", required, v-model="user.email")
    .form-group
      label(for="password") Ваш пароль (минимум 6 символов)
      input.form-control#password(type="password", placeholder="Введите пароль:", required, v-model="user.password", minlength="6")
    .form-group
      label(for="password2") Повторите пароль
      input.form-control#password2(type="password", placeholder="Повторите пароль:", required, v-model="user.confirmPassword", minlength="6")
    .alert.alert-danger(role="alert", v-if="error")
      strong Упс!
        |  Пароли не совпадают или вы забыли их ввести.
    button.btn.btn-primary(type="submit") Зарегистрироваться
</template>

<script>
export default {
  name: 'sign-up',
  data() {
    return {
      error: false,
      user: {
        email: '',
        password: '',
        confirmPassword: ''
      }
    }
  },
  methods: {
    registerUser() {
      if ((this.user.password !== this.user.confirmPassword) || this.user.password.length < 6) {
          this.error = true;
      } else {
          this.error = false;
          firebase.auth().createUserWithEmailAndPassword(this.user.email, this.user.password)
              .then( () => {
                  this.$emit('regSuccess', 'sign-in');
              })
              .catch( error => {
                  console.log(error);
              })
      }
    }
  }
}
</script>
