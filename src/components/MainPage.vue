<template lang="pug">
div
    form.input-group.mt-5
        input.form-control(type="text", placeholder="Добавь сериал...")
        span.input-group-btn
            button.btn.btn-secondary(type="submit") Добавить!
    ol.mt-5
    .mt-5
        button.btn.btn-primary Выбрать случайный сериал
        h2.mt-3.mb-3
        button.btn.btn-primary Выбранный сериал просмотрен
</template>

<script>
    export default  {
        name: 'main-page',
        data() {
            return {

            }
        }
    }
</script>