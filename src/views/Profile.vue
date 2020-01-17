<template>
    <div>
        <v-row class="text-left">
            <v-col cols="2">
                <img :src="'https://randomuser.me/api/portraits/men/' + $route.params.id + '.jpg'" style="max-width: 100%">
            </v-col>
            <v-col cols="10" class="text-left">
                <h1>{{userData.name}}</h1>
                <p>
                    Веб-сайт: <a :href="'http://'+ userData.website" target="_blank">{{userData.website}}</a>
                </p>
                <p>
                    E-mail: <a :href="'mailto:' + userData.email">{{userData.email}}</a>
                </p>
                <p>
                    Город: {{userData.address.city}}
                </p>
                <p>
                    Место работы: {{userData.company.name}}
                </p>
            </v-col>
        </v-row>
        
        <v-divider></v-divider>

        <div class="mt-5">
            <h2 class="text-left mb-7">Публикации</h2>

            <v-card 
                v-for="post in posts" :key="post.id"
                max-width="1000px" class="text-left pa-3 mb-8" >
                <v-list-item>
                    <v-list-item-avatar color="grey">
                        <img :src="'https://randomuser.me/api/portraits/men/' + post.userId + '.jpg'">
                    </v-list-item-avatar>
                    <v-list-item-content>
                        <v-list-item-title>{{post.title}}</v-list-item-title>
                        <v-list-item-subtitle>by {{userData.name}}</v-list-item-subtitle>
                    </v-list-item-content>
                </v-list-item>
                <v-card-text>{{post.body}}</v-card-text>
                <v-card-actions>
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

        </div>
    </div>
</template>

<script>
export default {
    name: 'Profile',
    data(){
        return {
            posts: [],
            userData: {
                name: '',
                website: '',
                email: '',
                address: '',
                company: {
                    name: ''
                }
            }
        } 
    },
    methods: {
        getPosts(){
            this.axios.get('http://188.225.47.187/api/jsonplaceholder.php?endpoint=posts?userId=' + this.$route.params.id)
                .then((response)=>{
                    this.posts = response.data;
                })
        },
        getUserData(){
            this.axios.get('http://188.225.47.187/api/jsonplaceholder.php?endpoint=users/' + this.$route.params.id)
                .then((response)=>{
                    this.userData = response.data;
                    window.console.log(this.userData);
                })
        }
    },
    mounted(){
        this.getPosts();
        this.getUserData();
    },
    watch: {
        $route() {
            this.getPosts();
            this.getUserData();
        }
    }
}
</script>