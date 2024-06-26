<template>
  <v-app-bar :elevation="1" >
    <v-app-bar-nav-icon @click.stop="showDrawerMenu = !showDrawerMenu"></v-app-bar-nav-icon>
    <v-app-bar-title>
      <span style="cursor:pointer" @click="$router.push('/')">
        <h4><img src="/pa-logo-192.png" height="32" width="32" style="vertical-align:bottom">  {{ APP_NAME }}</h4>
      </span>
    </v-app-bar-title>
    <template v-if="!username" v-slot:append>
      <v-btn v-if="!$vuetify.display.smAndUp" icon="mdi-login" to="/sign-in" :aria-label="$t('Header.SignIn')"></v-btn>
      <v-btn v-else prepend-icon="mdi-login" to="/sign-in" :aria-label="$t('Header.SignIn')">{{ $t('Header.SignIn') }}</v-btn>
    </template>
    <template v-else v-slot:append>
      <v-menu scroll-strategy="close">
        <template v-slot:activator="{ props }">
          <v-btn v-if="!$vuetify.display.smAndUp" v-bind="props" icon="mdi-account-circle"></v-btn>
          <v-btn v-else v-bind="props" class="text-lowercase" prepend-icon="mdi-account-circle">{{ username }}</v-btn>
        </template>
        <v-list>
          <v-list-item class="d-sm-none" :slim="true" prepend-icon="mdi-account" disabled>{{ username }}</v-list-item>
          <v-divider class="d-sm-none"></v-divider>
          <v-list-item :slim="true" prepend-icon="mdi-view-dashboard-outline" to="/dashboard" :aria-label="$t('Header.Dashboard')">{{ $t('Header.Dashboard') }}</v-list-item>
          <v-list-item :slim="true" prepend-icon="mdi-cog-outline" to="/settings" :aria-label="$t('Header.Settings')">{{ $t('Header.Settings') }}</v-list-item>
          <v-list-item :slim="true" prepend-icon="mdi-logout" @click="signOut" :aria-label="$t('Header.SignOut')">{{ $t('Header.SignOut') }}</v-list-item>
        </v-list>
      </v-menu>
    </template>
  </v-app-bar>

  <v-navigation-drawer v-model="showDrawerMenu" temporary>
    <v-list :items="getDrawerMenuItems"></v-list>
  </v-navigation-drawer>
</template>

<style scoped>
  .v-app-bar.v-toolbar {
  background-color: #75AADB;
}
</style>

<script>
import { mapStores } from 'pinia'
import { useAppStore } from '../store'
import constants from '../constants'

export default {
  data() {
    return {
      APP_NAME: constants.APP_NAME,
      showDrawerMenu: false,
      showProfileMenu: false,
    }
  },
  computed: {
    ...mapStores(useAppStore),
    username() {
      return this.appStore.user.username
    },
    getDrawerMenuItems() {
      return this.$router.options.routes
        .filter(r => r.meta && r.meta.drawerMenu)
        .filter(r => this.username ? r.meta.requiresAuth !== false : !r.meta.requiresAuth)
        .map((r => ({ title: this.$t(`Router.${r.meta.title}.Title`), props: { 'prepend-icon': r.meta.icon, 'base-color': r.meta.color, to: r.path }})))
    }
  },
  methods: {
    signOut() {
      this.appStore.signOut()
      this.$router.push({ path: '/' })
    }
  },
}
</script>
