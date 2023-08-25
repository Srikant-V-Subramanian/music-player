<script setup>
let volume = 1;
let current = {};
let index = 0;
let isPlaying = false;
let songs = [
  {
    title: "Better Now",
    artists: "Post Malone",
    src: "../src/assets/Post-malone-better-now.mp3",
  },
  {
    title: "OTP",
    artists: "London View",
    src: "../src/assets/otp-london-view.mp3",
  },
  {
    title: "Rockstar",
    artists: "Post Malone ft. 21 Savage",
    src: "../src/assets/Post-malone-rockstar.mp3",
  },
  {
    title: "Bye Bye",
    artists: "Marshmello, Juice WRLD",
    src: "../src/assets/jWRLD-marshmello-bye-bye.mp3",
  },
  {
    title: "BUTTERFLY EFFECT",
    artists: "Travis Scott",
    src: "../src/assets/travis-scott-butterfly-effect.mp3",
  },
  {
    title: "Never Fail",
    artists: "Morray ft. bunny the butcher",
    src: "../src/assets/morray-never-fail.mp3",
  },
];

let player = new Audio();

function volumeHandler() {
  const volumeSlider = document.getElementById("volume");
  player.volume = volumeSlider.value;
  volume = player.volume;
  console.log(player.volume);
}
function duration() {
  const songDuration = Math.round((100 * player.duration) / 60) / 100;
  console.log(songDuration);
}
function play(song) {
  if (typeof song.src != "undefined") {
    current = song;
    player.src = current.src;
  }
  player.play();
  player.addEventListener(
    "ended",
    function () {
      index++;
      if (index > songs.length - 1) {
        index = 0;
      }
      current = songs[index];
      play(current);
    }.bind(this)
  );
  isPlaying = true;
}

function pause() {
  player.pause();
  isPlaying = false;
}

function next() {
  index++;
  if (index > songs.length - 1) {
    index = 0;
  }
  current = songs[index];
  player.src = current.src;
  if (isPlaying) {
    player.pause();
    player.play();
  }
}

function prev() {
  index--;
  if (index < 0) {
    index = songs.length - 1;
  }
  current = songs[index];
  player.src = current.src;
  if (isPlaying) {
    player.pause();
    player.play();
  }
}

function created() {
  current = songs[index];
  player.src = current.src;
}
</script>

<template>
  <header>
    <h1>A music player</h1>
  </header>
  <main>
    <section class="player">
      <h2 class="song-title">
        {{ current.title }} - <span>{{ current.artists }}</span>
      </h2>
      <section class="controls">
        <input
          type="range"
          name="volume"
          id="volume"
          min="0"
          max="1"
          value="1"
          step="0.02"
          @change="volumeHandler"
        />
        <p>Volume : {{ volume * 100 }}</p>
        <button class="prev" @click="prev">Prev</button>
        <button class="play" v-if="!isPlaying" @click="play">Play</button>
        <button class="pause" v-else @click="pause">Pause</button>
        <button class="next" @click="next">Next</button>
        <button @click="duration">Duration</button>
        <h3>Volume Slider</h3>
      </section>
    </section>
    <section class="playlist">
      <h3>The Playlist</h3>
      <button
        v-for="song in songs"
        :key="song.src"
        @click="play(song)"
        :class="song.src == current.src ? 'song playing' : 'song'"
      >
        {{ song.title }} - {{ song.artists }}
      </button>
    </section>
  </main>
  <footer>
    <h1>CHECK THE CONSOLE!!</h1>
  </footer>
</template>
