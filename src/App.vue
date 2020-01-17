<template>
  <v-app id="app">
    <!-- <v-app-bar color="green" hide-on-scroll dark app>
        <img src="/img/logo.png" height="45px" class="mr-3">
        <v-toolbar-title>SocialApp</v-toolbar-title>
        <v-spacer></v-spacer>
        <div id="nav">
          <router-link to="/">Home</router-link> |
          <router-link to="/profile">Профиль</router-link>
        </div>
    </v-app-bar> -->
    
    <v-navigation-drawer
        color="green"
        dark
        expand-on-hover
        hide-overlay
        permanent
        right
        app
      >
        <v-list
          nav
          shaped
          dense
        >
          <v-list-item two-line>
            <v-list-item-avatar>
              <img :src="'https://randomuser.me/api/portraits/men/' + myId + '.jpg'">
            </v-list-item-avatar>

            <v-list-item-content class="text-left">
              <v-list-item-title class="font-weight-black">SocialLink</v-list-item-title>
              <v-list-item-subtitle>Leanne Graham</v-list-item-subtitle>
            </v-list-item-content>
          </v-list-item>

          <v-divider class="my-3"></v-divider>

          <v-list-item link :to="'/'">
            <v-list-item-icon>
              <v-icon>mdi-home-outline</v-icon>
            </v-list-item-icon>
            <v-list-item-content>
              <v-list-item-title class="text-left">Главная</v-list-item-title>
            </v-list-item-content>
          </v-list-item>

          <v-list-item link :to="'/users/' + myId">
            <v-list-item-icon>
              <v-icon>mdi-account-outline</v-icon>
            </v-list-item-icon>
            <v-list-item-content>
              <v-list-item-title class="text-left">Мой профиль</v-list-item-title>
            </v-list-item-content>
          </v-list-item>

          <v-list-item link :to="'/users'" exact>
            <v-list-item-icon>
              <v-icon>mdi-account-multiple-plus-outline</v-icon>
            </v-list-item-icon>
            <v-list-item-content>
              <v-list-item-title class="text-left">Найти друзей</v-list-item-title>
            </v-list-item-content>
          </v-list-item>

        </v-list>
      </v-navigation-drawer>
    
    <v-content class="px-12 py-3" >
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
      myId: 6
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
</style>
