<template>
  <header>
    <h1>Music App</h1>
  </header>
  <main>
    <section class="player">
      <h2 class="song-title">{{ current.title }} - <span>{{ current.artists }}</span></h2>
      <section class="controls">
        <button class="prev" @click="prev">Prev</button>
        <button class="play" v-if="!isPlaying" @click="play">Play</button>
        <button class="pause" v-else @click="pause">Pause</button>
        <button class="next" @click="next">Next</button>
      </section>
    </section>
    <section class="playlist">
        <h3>The Playlist</h3>
        <button v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src == current.src) ? 'song playing' : 'song'">
          {{ song.title }} - {{ song.artists }}
        </button>
      </section>
  </main>

</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs: [
        {
          title: 'Better Now',
          artists: 'Post Malone',
          src: require("../src/assets/Post-malone-better-now.mp3")
        },
        {
          title: 'Rockstar',
          artists: 'Post Malone ft. 21 Savage',
          src: require("../src/assets/Post-malone-rockstar.mp3")
        }
      ],
      player: new Audio()
    }
  },
  methods: {
    play (song) {
      if (typeof song.src != "undefined") {
        this.current = song;
        this.player.src = this.current.src;
      }
      this.player.play();
      this.player.addEventListener('ended', function () {
        this.index++;
        if (this.index > this.songs.length - 1) {
          this.index = 0;
        }
        this.current = this.songs[this.index];
        this.play(this.current);
      }.bind(this));
      this.isPlaying = true;
    },
    pause() {
      this.player.pause()
      this.isPlaying = false
    },
    next() {
      this.index++
      if (this.index > this.songs.length - 1) {
        this.index = 0
      }
      this.current = this.songs[this.index]
      this.play(this.cuurent)
    },
    prev() {
      this.index--
      if (this.index < 0) {
        this.index = this.songs.length - 1
      }
      this.current = this.songs[this.index]
      this.play(this.cuurent)
    }
  }, 

  created() {
    this.current = this.songs[this.index]
    this.player.src = this.current.src
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: sans-serif;
}

.player {
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 3rem;
}

button{
  background-color: rgb(176, 48, 70);
  color: white;
  padding: 0.5rem;
  border-radius: 2px;
  margin: 5px;
  border: none;
  text-align: center;
  cursor: pointer;
  transition-duration: 0.4s;
  border: 2px solid;

}

button:hover{
  background-color: white;
  color: black;
  border-color: brown;
  border: 2px solid #4CAF50;

}

.playlist {
  display: flex;
  justify-content: center;
  align-items: center;
}

header{
  display: flex;
  justify-content: center;
  align-items: center;
  font-family: sans-serif;
  background-color: rgb(33, 33, 33);
  color: white;
  height: 10vh;
}
</style>