<template>
    <div class="d-flex justify-center">
        <v-card width="600px" class="mt-12 pa-10">
            <v-card-title>
                Войдите в аккаунт
            </v-card-title>
            <v-text-field
                label="Введите логин"
                v-model="login"
                outlined
            ></v-text-field>

            <v-text-field
                label="Введите пароль"
                v-model="password"
                outlined
            ></v-text-field>

            <v-btn @click="authenticate">
                Войти
            </v-btn>
        </v-card>
    </div>
</template>

<script>
export default {
    name: 'Login',
    data(){
        return {
            login: '',
            password: ''
        }
    },
    methods: {
        authenticate(){
            this.axios.get('http://188.225.47.187/api/jsonstorage/d3cec8d60e3625b856a50c0722f2c8ab')
                .then(
                    (response) => {
                        let users = response.data;
                        let found = false;

                        for(let index in users){
                            if(this.login == users[index].login && this.password == users[index].password){

                                this.$emit('login', {
                                  id: index,
                                  photo: users[index].photo,
                                  name: users[index].name
                                });

                                this.$router.push('/users/' + index);
                                found = true;
                                break;
                            }
                        }
                        if(!found) window.alert('Неверный логин или пароль');
                    }
                )
        }
    }
}
</script>