<template lang="pug">
div
    form.input-group.mt-5
        input.form-control(type="text", placeholder="Добавь сериал...", v-model="title")
        span.input-group-btn
            button.btn.btn-secondary(@click.prevent="addSerial") Добавить!
    ol.mt-5
        li(v-for="(serial, index) in data.serials", :key="serial", @click="removeSerial(index)") {{serial}}
    .mt-5
        button.btn.btn-primary(@click="chooseRandomSerial(0, data.serials.length)") Выбрать случайный сериал
        h2.mt-3.mb-3 {{data.currentSerial}}
        button.btn.btn-primary(@click="removeCurrentSerial") Выбранный сериал просмотрен
</template>

<script>
    export default  {
        name: 'main-page',
        data() {
            return {
                title: '',
                data: {
                    serials: [],
                    currentSerial: 'Сериал не выбран...',
                    numberOfCurrentSerial: null
                }
            }
        },
        props: ['uid'],
        methods: {
            addSerial() {
                if(this.title) {
                    this.data.serials.push(this.title);
                }
                firebase.database().ref('users/' + this.uid).set({
                    data: this.data
                });
                this.title = ''
            },
            removeSerial(index) {
                this.data.serials.splice(index, 1);
                if(this.data.numberOfCurrentSerial === index) {
                    this.data.currentSerial = 'Сериал не выбран...';
                    this.data.numberOfCurrentSerial = null;
                }
                for (let i = 0; i < this.data.serials.length; i++) {
                    if(this.data.currentSerial === this.data.serials[i]) {
                        this.data.numberOfCurrentSerial = i;
                    }
                }
                firebase.database().ref('users/' + this.uid).set({
                    data: this.data
                });
            },
            chooseRandomSerial(min, max) {
                const random = Math.floor(Math.random() * (max-min) + min);
                if(this.data.serials.length > 0) {
                    this.data.currentSerial = this.data.serials[random];
                    this.data.numberOfCurrentSerial = random;
                }
                firebase.database().ref('users/' + this.uid).set({
                    data: this.data
                });
            },
            removeCurrentSerial() {
                if(this.data.numberOfCurrentSerial !== null && this.data.numberOfCurrentSerial) {
                    this.data.serials.splice(this.data.numberOfCurrentSerial, 1);
                    this.data.currentSerial = 'Сериал не выбран...';
                    this.data.numberOfCurrentSerial = null;
                }
                firebase.database().ref('users/' + this.uid).set({
                    data: this.data
                });
            }
        },
        created() {
            this.data.serials = [];
            const takeSerials = firebase.database().ref('users/' + this.uid + '/data');
            takeSerials.on('value', (snapshot) => {
                if (snapshot.val().currentSerial && snapshot.val().serials) {
                    this.data = snapshot.val()
                } else {
                    this.data.serials = [];
                    this.data.currentSerial= 'Сериал не выбран...'
                }
            })
        }
    }
</script>