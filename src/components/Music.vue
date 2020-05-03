<template>
  <div>
    <div class="Player">
      <h2 class="song-title">
        {{current.songTitle}} -
        <span>{{current.artist}}</span>
      </h2>
      <div class="control">
        <button class="prev" @click="prev">Prev</button>
        <button class="play" v-if="!isPlay" @click="play">Play</button>
        <button class="pause" v-else @click="pause">Pause</button>
        <button class="next" @click="next">Next</button>
      </div>
    </div>
    <div class="playlist">
      <h3>The Playlist</h3>
      <button
        v-for="song in songs"
        :key="song.src"
        @click="play(song)"
        :class="(song.src == current.src) ? 'song playing' : 'song'"
      >{{ song.songTitle}} - {{song.artist}}</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "Music",
  data() {
    return {
      current: {},
      index: 0,
      isPlay: false,
      songs: [
        {
          songTitle: "Grateful",
          artist: "Neffex",
          src: require("../assets/neffex-grateful.mp3")
        },
        {
          songTitle: "Invincible",
          artist: "Deaf Kev",
          src: require("../assets/deaf-kev-invincible.mp3")
        }
      ],
      player: new Audio()
    };
  },
  methods: {
    play(song) {
      if (typeof song.src != "undefined") {
        this.current = song;
        this.player.src = this.current.src;
      }
      this.player.play();
      this.isPlay = true;
    },
    pause() {
      this.player.pause();
      this.isPlay = false;
    },
    prev() {
      this.index--;
      if (this.index < 0) {
        this.index = this.songs.length - 1;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    },
    next() {
      this.index++;
      if (this.index > this.songs.length - 1) {
        this.index = 0;
      }
      this.current = this.songs[this.index];
      this.play(this.current);
    }
  },
  created() {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
    // this.player.play();
  },
  props: {}
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
