<template>
    <div>

        <!-- Заголовочная часть страницы -->
        <v-row>
            <!--  cols="10" чтобы ограничить ширину и не занимать крайнюю правую область
                  text-left - класс для выравнивания по левому краю -->
            <v-col cols="10" class="text-left">
                <h1 class="green--text text--darken-2">
                    <!-- Иконка (человечек) -->
                    <v-icon large color="green darken-2">mdi-account-outline</v-icon>
                    <!-- Имя пользователя -->
                    {{userData.name}}
                </h1>
            </v-col>
        </v-row>




        <!-- Подробная информация -->
        <v-row class="text-left">
            <!-- Колонка с фотографией -->
            <v-col cols="2">
                <img :src="userData.photo" style="max-width: 100%">
            </v-col>


            <!-- Колонка с полной информацией -->
            <v-col cols="10" class="text-left">
                <p>
                    Веб-сайт: <a :href="'http://'+ userData.website" target="_blank">{{userData.website}}</a>
                </p>
                <p>
                    E-mail: <a :href="'mailto:' + userData.email">{{userData.email}}</a>
                </p>
                <p>
                    Город: {{userData.city}}
                </p>
                <p>
                    Место работы: {{userData.company}}
                </p>
            </v-col>
        </v-row>




        <!-- Линия-разделитель -->
        <v-divider></v-divider>




        <!-- Раздел с постами пользователя -->
        <div class="mt-5">
            <h2 class="text-left mb-7">Публикации</h2>


            <!-- НАЧАЛО: Раздел для создания нового поста (виден только в своем профиле [v-if]) -->
            <!-- myId мы получаем от App.vue через props -->
            <v-row v-if="$route.params.id == myId" class="text-left mb-10">
                <v-col cols="12" sm="10">
                    <v-text-field outlined label="Заголовок" v-model="newPostTitle"></v-text-field>
                    <v-textarea outlined label="Текст" v-model="newPostBody"></v-textarea>
                    <v-btn @click="send">Опубликовать</v-btn>
                </v-col>
            </v-row>
            <!-- КОНЕЦ: Раздел для создания нового поста (виден только в своем профиле [v-if]) -->


            <!-- НАЧАЛО: Раздел с постами пользователя -->
            <v-row>

                <!-- Карточка поста -->
                <v-col cols="12" sm="10" v-for="post in posts" :key="post.body">
                    <v-card max-width="1000px" class="text-left pa-3 mb-8">
                        <!-- Шапка поста -->
                        <v-list-item>
                            <!-- Выводим аватарку -->
                            <v-list-item-avatar>
                                <img :src="userData.photo">
                            </v-list-item-avatar>
                            <!-- Выводим имя и заголовок поста -->
                            <v-list-item-content>
                                <v-list-item-title class="font-weight-bold">{{post.title}}</v-list-item-title>
                                <v-list-item-subtitle>автор {{userData.name}}</v-list-item-subtitle>
                            </v-list-item-content>
                        </v-list-item>

                        <!-- Текст поста -->
                        <v-card-text class="pl-12 body-1">
                            <!-- pre нужен для сохранения форматирования (пробелы и переносы строк) -->
                            <pre style="white-space: pre-wrap; "> {{post.body}} </pre>
                        </v-card-text>

                        <!-- ПОДВАЛ поста (лайк, поделиться и т.п.) -->
                        <v-card-actions>
                            <!-- занимает всю доступную ширину и прижимает внопки вправо! -->
                            <v-spacer></v-spacer>
        
                            <v-btn icon>
                                <v-icon>mdi-heart</v-icon>
                            </v-btn>
                            <v-btn icon>
                                <v-icon>mdi-bookmark</v-icon>
                            </v-btn>
                            <v-btn icon>
                                <v-icon>mdi-share-variant</v-icon>
                            </v-btn>
                        </v-card-actions>
                    </v-card>
                </v-col>

            </v-row>
            <!-- КОНЕЦ: Раздел с постами пользователя -->

        </div>
    </div>
</template>




<script>
export default {
    name: 'Profile',

    // Пропс нужен, чтобы принять myId из App.vue (он единственный, кто запоминает, под кем юзер залогинился)
    props: ["myId"],

    data(){
        return {
            posts: [],
            userData: {},

            // Почему сразу не создать объект newPost и не подвязаться к его свойствам?
            // Потому что в таком случае мы потеряем реактивность. Поэтому 2 отдельных строчных свойства,
            // объект из них соберем позже, в момент отправки
            newPostTitle: '',
            newPostBody: ''
        } 
    },
    methods: {
        // Получить все посты
        getPosts(){
            this.axios.get('http://188.225.47.187/api/jsonstorage/14e95d05fb612ec78414e2c27e2ca5b2')
                .then((response)=>{
                    // Цикл для фильтрации. 
                    for(let post of response.data){
                        // Берем только те посты, у которых userId совпадает с текущим Id
                        if(post.userId == this.$route.params.id)
                            // unshift - это добавление в начало
                            // для того, чтобы более новые посты были вверху списка
                            this.posts.unshift(post);
                    }
                })
        },

        // Получить информацию о текущем пользователе
        getUserData(){
            this.axios.get('http://188.225.47.187/api/jsonstorage/d3cec8d60e3625b856a50c0722f2c8ab')
                .then((response)=>{
                    // Получаем массив всех пользователей
                    let userList = response.data;
                    // Извлекаем информацию из массива по индексу (id пользователя)
                    this.userData = userList[this.$route.params.id]
                })
        },

        // Отправляем новый пост
        send(){
            // Для начала получим весь актуальный JSON постов
            this.axios.get('http://188.225.47.187/api/jsonstorage/14e95d05fb612ec78414e2c27e2ca5b2')
                .then((response)=>{
                    // тут у нас будет массив всех постов на текущий момент
                    let posts = response.data;

                    // Создаем объект для нового поста, его будем добавлять в массив,
                    // чтобы опубликовать пост
                    let newPost = {
                        userId: this.myId,
                        title: this.newPostTitle,
                        body: this.newPostBody
                    }

                    // добавим в полученный массив новый пост
                    posts.push(newPost);

                    // отправим обновленный массив всех постов на сервер для хранения
                    this.axios.put('http://188.225.47.187/api/jsonstorage/14e95d05fb612ec78414e2c27e2ca5b2', posts);

                    // добавим пост в массив постов, отображаемых на странице
                    // unshift добавляет в начало массива
                    this.posts.unshift(newPost);
                    
                    // сбросим поля ввода поста
                    this.newPostTitle = '';
                    this.newPostBody = '';
                })
        }
    },

    // При загрузке страницы
    mounted(){
        // Вызываем методы, описанные выше

        // Получаем все посты
        this.getPosts();
        // Получаем пользовательские данные
        this.getUserData();
    },

    // При обновлении маршрута посредством Vue Router
    watch: {
        $route() {
            this.getPosts();
            this.getUserData();
        }
    }
}
</script>