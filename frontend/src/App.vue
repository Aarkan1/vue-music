<template>
    <!--  this div will contain the YouTube Player
        when script gets loaded -->
  <div id="yt-player"></div>

  <br>
  <input v-model="videoId" type="text" placeholder="videoId">
  <button @click="playSong">Play song</button>

  <HelloWorld msg="Hello Vue 3.0 + Vite" />
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'

export default {
  name: 'App',
  components: {
    HelloWorld
  },
  data() {
    return {
      player: null,
      videoId: ''
    }
  },
  async mounted() {
    let searchString = 'the quiet'
    let res = await fetch('/api/yt/songs/' + searchString)
    let songs = await res.json()

    console.log(songs);

    this.player = new YT.Player('yt-player', {
      height: '300',
      width: '400',
      events: {
        'onStateChange': this.onPlayerStateChange
      }
    });
  },
  methods: {
    playSong() {
      this.player.loadVideoById(this.videoId)
    },
    onPlayerStateChange(event) {
      if (event.data != YT.PlayerState.PLAYING) return
    }
  }
}
</script>
