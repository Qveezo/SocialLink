<template>
  <v-app id="app">
    <v-navigation-drawer
    color="blue darken-1"
    dark
    expand-on-hover
    hide-overlay
    permanent
    left
    app
    >
    <v-list
    nav 
    shaped
    dense
    >
    <v-list-item link :to="Profile">
      <v-list-item-icon>
        <img src="https://randomuser.me/api/portraits/men/1.jpg" alt="Men" style="width: 35px; height: 35px;">
      </v-list-item-icon>
      <v-list-item-content>
      <v-list-item-title class="font-weight-black">
        SocialLink
      </v-list-item-title>
      <v-list-item-subtitle>
        Valeriy Anikin
      </v-list-item-subtitle>
    </v-list-item-content>
    </v-list-item>


    <v-divider class="my-3"></v-divider>



    <v-list-item link :to="pyt">
      <v-list-item-icon>
        <v-icon>mdi-home-outline</v-icon>
      </v-list-item-icon>
      <v-list-item-content>
      <v-list-item-title class="text-left">
        Главная
      </v-list-item-title>
    </v-list-item-content>
    </v-list-item>


    <v-list-item link :to="profile">
      <v-list-item-icon>
        <v-icon> mdi-face-man</v-icon>
      </v-list-item-icon>
      <v-list-item-content>
      <v-list-item-title class="text-left">
        Мой Профиль
      </v-list-item-title>
    </v-list-item-content>
    </v-list-item>


    <v-list-item link :to="profile">
      <v-list-item-icon>
        <v-icon>mdi-account-plus-outline</v-icon>
      </v-list-item-icon>
      <v-list-item-content>
      <v-list-item-title class="text-left">
        Найти Друзей
      </v-list-item-title>
    </v-list-item-content>
    </v-list-item>




    </v-list>

      
    </v-navigation-drawer>
    <v-content class="px-12 py-3">
      <v-container fluid>
        <router-view/>
      </v-container>
    </v-content>
  </v-app>
</template>




<script>
export default {
  name: 'App',
  data(){
    return {
      // Это данные для отображения в боковой панели
      // мы будем их фиксировать в момент аутентификации
      myName: '',
      myId: '',
      myPhoto: ''
    }
  },
  methods: {
    // Метод для обработки события аутентификации (пользователь вошел в аккаунт)
    // Событие возникает на router-view и генерируется в src/views/Login.vue при помощи $emit
    setUser(data){
      this.myId = data.id;
      this.myName = data.name;
      this.myPhoto = data.photo;
    }
  }
}
</script>




<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

#nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #fff;
    text-decoration: none;

    &.router-link-exact-active {
      color: #99ffd3;
      text-decoration: underline;
    }
  }
}

a {
  text-decoration:none;
  color: #FFFFFF !important;
}
</style>
