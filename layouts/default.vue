<template>
  <v-app>
    <v-navigation-drawer v-model="drawer" app right>
      <v-toolbar flat>
        <v-list>
          <v-list-tile>
            <v-list-tile-title class="title">Main Menu</v-list-tile-title>
          </v-list-tile>
        </v-list>
      </v-toolbar>
      <v-divider></v-divider>
      <v-list dense class="pt-0">
        <v-list-tile v-for="item in items" :key="item.title">
          <v-list-tile-action>
            <v-icon>{{ item.icon }}</v-icon>
          </v-list-tile-action>
          <v-list-tile-content>
            <v-list-tile-title><nuxt-link :to="item.url">{{ item.title }}</nuxt-link></v-list-tile-title>
          </v-list-tile-content>
        </v-list-tile>
      </v-list>
    </v-navigation-drawer>

    <v-toolbar app>
      <v-toolbar-title @click="homeClick" style="cursor: pointer"><v-icon class="info--text">home</v-icon> ScoreInput</v-toolbar-title>
      <v-spacer/>
      <v-toolbar-side-icon @click.stop="drawer = !drawer"/>
    </v-toolbar>
    <v-content>
      <v-container>
        <nuxt/>
      </v-container>
    </v-content>
  </v-app>
</template>

<script>
export default {
    data () {
      return {
        items: [
          { title: 'Home', icon: 'home', url: '/' },
          { title: 'Teacher', icon: 'group', url: '/teacher' },
          { title: 'Classroom', icon: 'group', url: '/classroom' },
          { title: 'Subject', icon: 'group', url: '/subject' },
          { title: 'Student', icon: 'group', url: '/student' },
          { title: 'Mark', icon: 'group', url: '/mark' },
          { title: 'Chat', icon: 'group', url: '/chat' },
        ],
        right: null
      }
    },  

    //สำหรับ login
    // created() {
    // let userObj = JSON.parse(window.sessionStorage.getItem('user') || {})
    // if (!userObj.user) {
    //     return this.$router.replace('/login')
    // }
// },  

  computed: {
    online: {
      get() {
        return this.$store.state.online
      },
      set(value) {
        this.$store.commit('setOnline', value)
      },
    },
    drawer: {
      get() {
        return this.$store.state.drawer
      },
      set(value) {
        this.$store.commit('setDrawer', value)
      },
    },
  },

  mounted() {
    this.$store.commit('setOnline', window.navigator.onLine)
    window.addEventListener('offline', this.toggleNetworkStatus)
    window.addEventListener('online', this.toggleNetworkStatus)
  },

  beforeDestroyed() {
    window.removeEventListener('offline', this.toggleNetworkStatus)
    window.removeEventListener('online', this.toggleNetworkStatus)
  },

  methods: {
    toggleNetworkStatus({ type }) {
      this.online = type === 'online'
    },
    homeClick() {
      this.$router.replace("/")
    },
  },
}
</script>
