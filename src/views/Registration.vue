<template>
    <div class="d-flex justify-center">
        <v-card width="600px" class="mt-12 pa-10">
            <v-card-title>
                Регистрация
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

            <v-text-field
                label="Введите полное имя"
                v-model="name"
                outlined
            ></v-text-field>

            <v-text-field
                label="Введите email"
                v-model="email"
                outlined
            ></v-text-field>

            <v-text-field
                label="Введите веб-сайт"
                v-model="website"
                outlined
            ></v-text-field>

            <v-text-field
                label="Введите город"
                v-model="city"
                outlined
            ></v-text-field>

            <v-text-field
                label="Где Вы работаете"
                v-model="company"
                outlined
            ></v-text-field>

            <v-text-field
                label="Введите ссылку на фото"
                v-model="photo"
                outlined
            ></v-text-field>

            <v-btn @click="register">
                Зарегистрироваться
            </v-btn>
        </v-card>
    </div>
</template>

<script>
export default {
    name: 'Registration',
    data(){
        return {
            // Свойства для реактивной привязки текстовых полей
            login: '',
            password: '',
            name: '',
            website: '',
            email: '',
            city: '',
            company: '',
            photo: ''
        }
    },

    methods: {

        register(){
            // Делаем запрос на сервер для получения списка всех пользователей
            this.axios.get('http://188.225.47.187/api/jsonstorage/d3cec8d60e3625b856a50c0722f2c8ab')
                .then( (response)=>{
                    // Это список всех зарегистрированных пользователей
                    let userList = response.data;

                    // Создаем нового пользователя в том же формате, что остальные
                    let newUser = {
                        login: this.login,
                        password: this.password,
                        name: this.name,
                        website: this.website,
                        email: this.email,
                        city: this.city,
                        company: this.company,
                        photo: this.photo
                    }

                    // Добавим его в массив пользователей
                    userList.push(newUser);

                    // Отправим обновленный массив пользователей на сервер
                    this.axios.put('http://188.225.47.187/api/jsonstorage/d3cec8d60e3625b856a50c0722f2c8ab', userList)
                        .then( 
                            (response)=>{
                                // Если получилось, переводим на страницу входа
                                if(response.data == "ok")
                                    this.$router.push('/login');
                                else
                                    window.alert('Что-то пошло не так!');
                            } 
                        );
                } )
        }
    }
}
</script>