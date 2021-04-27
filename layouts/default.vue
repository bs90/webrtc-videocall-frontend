<template>
  <v-app>
    <v-app-bar
      :clipped-left="clipped"
      fixed
      app
      flat
    >
      <img src="../assets/video.png" alt="logo" style="width: 100px; padding: 30px 0;">
      <v-toolbar-title v-text="title" style="font-size: 15px;margin-bottom: -3px;font-weight: bold;color: #eb7934;"/>
      <v-spacer />
    </v-app-bar>
    <v-content>
      <div class="main">
        <nuxt />
      </div>
    </v-content>
    <v-overlay :value="overlay">
      <p>
        <v-progress-circular
          indeterminate
          color="primary"
        />
        Lost connection to server, trying to reconnect ...
      </p>
    </v-overlay>
  </v-app>
</template>

<style>
  .main {
    height: calc(100vh - 64px);
    width: 100vw;
    overflow-y: hidden;
  }
</style>

<script>
import { mapState } from 'vuex'

export default {
  data () {
    return {
      clipped: false,
      title: '　ビデオチャット'
    }
  },
  computed: mapState({
    name: state => state.setting.name,
    overlay: state => state.setting.overlay
  }),
  mounted () {
    this.$socket.on('reconnecting', () => {
      this.$store.commit('setting/setOverlay', {
        overlay: true
      })
    })

    this.$socket.on('reconnect', () => {
      this.$store.commit('setting/setOverlay', {
        overlay: false
      })
    })

    this.$socket.on('reconnect', () => {
      this.$store.commit('setting/setOverlay', {
        overlay: false
      })
    })

    this.$socket.on('reject', (data) => {
      this.$router.push('/error')
    })
  }
}
</script>
