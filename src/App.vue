<template>
  <div id="app">
    <header>
      <h1>My Music</h1>
    </header>
    <main>
      <section>
          <h2 class="song-title">{{ current.title }} - <span>{{ current.artist }}</span> </h2>
          <div class="controls">
            <button class="prev" @click="prev" >Prev</button>
            <button class="play" v-if = "!isPlaying" @click= "play" > Play </button>
            <button class="pause" v-else @click = "pause" > Pause </button>
            <button class="next" @click = "next" > Next </button>
          </div>
      </section>
      <section class="playlist">
        <h3>Current Playlist</h3>
        <button v-for="song in songs" :key="song.src" @click="play(song)" :class="(song.src == current.src) ? 'song playing' : 'song' " >
          {{ song.title }} - {{ song.artist }}
        </button>
      </section>
    </main>
  </div>
</template>

<script>

export default {
  name: 'App',
  data () {
    return {
      current: {},
      index: 0,
      isPlaying: false,
      songs: [
        {
          title: 'Want it All',
          artist: 'Burna boy',
          src: require('./assets/BurnaBoy-Want-It-All.mp3')
        },
        {
          title: 'Fever',
          artist: 'Sefa',
          src: require('./assets/Sefa-Fever.mp3')
        },
        {
          title: 'Je-M’appelle',
          artist: 'Darkovibes',
          src: require('./assets/Darkovibes-Je-M’appelle.mp3')
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
      this.player.addEventListener('ended', function(){
        this.index++

        if (this.index > this.songs.length - 1) {
            this.index = 0;
        }

        this.current = this.songs[this.index];
        this.play(this.current);

      }.bind(this));
      this.isPlaying = true;
    },
    pause () {
      this.player.pause();
      this.isPlaying = false;
    },
    prev () {
      this.index--;
      if (this.index < 0) {
          this.index = this.songs.length - 1;
      }

      this.current = this.songs[this.index];
      this.play(this.current);
    },
    next () {
      this.index++;
      if (this.index > this.songs.length - 1) {
          this.index = 0;
      }

      this.current = this.songs[this.index];
      this.play(this.current);
    }
  },
  created () {
    this.current = this.songs[this.index];
    this.player.src = this.current.src;
    //this.player.play();
  }
}
</script>

<style>
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}
body {
  font-family: sans-serif;
}
header {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 15px;
  background-color: #fff;
}
main {
  width: 100%;
  max-width: 768px;
  margin: 0 auto;
  padding: 25px;
}
.song-title{
  color: #53565A;
  font-size: 32px;
  font-weight: 700;
  text-transform: uppercase;
  text-align: center;
}
.song-title span {
  font-weight: 400;
  font-style: italic;
}
.controls {
  display: flex;
  justify-content: center;
  padding: 30px 15px;
  align-items: center;
}
button {
  appearance: none;
  background: none;
  border: none;
  outline: none;
  cursor: none;
}
button:hover {
  opacity: 0.8;
}
.play {
  font-size: 20px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0px 15px;
  border-radius: 8px;
  color: #FFF;
  background-color: #CCAE33;
  cursor: pointer;
}
.next, .prev {
  font-size: 16px;
  font-weight: 700;
  padding: 10px 20px;
  margin: 0px 15px;
  border-radius: 6px;
  color: #FFF;
  background-color: #796e43;
  cursor: pointer;
}
.pause {
  font-size: 20px;
  font-weight: 700;
  padding: 15px 25px;
  margin: 0px 15px;
  border-radius: 8px;
  color: #FFF;
  background-color: #559242;
  cursor: pointer;
}
/* h3 {
  font-size: 22px;
  text-decoration: underline;
  padding: 10px;
} */
.playlist {
  padding: 0px 30px;
}
.playlist h3 {
  color: #262904;
  text-transform: uppercase;
  padding-bottom: 5px;
}
.playlist .song {
  display: block;
  width: 100%;
  padding: 15px;
  font-size: 20px;
  font-weight: 700;
  cursor: pointer;
}
.playlist .song:hover{
  color: #494D08;
}
.playlist .song.playing{
  color: #FFF;
  background-image: linear-gradient(to right, #80862d, #2e2d0b);
}

</style>
