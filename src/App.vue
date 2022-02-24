<template>
  <div>
   <header>
     <h1>My music app</h1>
   </header>
   <main>
      <div class="blok">
        <div class="player">
          <img  class="image" :src="songs.url">
          <h2 class="current-title">{{ current.title }}      
            <button class="fa-regular fa-heart" v-if="songs.isFavorite" @click="toggleFav"></button>
            <button class="fa-solid fa-heart" @click="toggleFav" v-else></button> 
         </h2>
         <h4 class="current-artist">{{ current.artist }}</h4>
         
        </div>
      </div>
      <div class="controls">
          <button class="prev fa-solid fa-backward-step"  @click="prev"></button>
          <button class="play fa-solid fa-play" v-if="!isPlaying" @click="play"></button>
          <button class="pause fa-solid fa-pause" @click="pause" v-else></button>
          <button class="next fa-solid fa-forward-step" @click="next"></button>
      </div>
      <h1 class="playlist">My playlist</h1>
      <div v-for="song in songs" :key="song.title" @click="play(song)" :class="(song.src == current.src) ? 'song playing' : 'song'">
       <img  class="artist-image" :src="song.url">
        <h2 class="song-title">{{ song.title }} </h2>
       <h4 class="song-artist">{{ song.artist }} <span>{{ song.time }}</span></h4>
       <hr>
      </div>
   </main>
  </div>
</template>

<script>
export default {
    name: "Music-App",
    data() {
        return {
            current: [],
            index: 0,
            isPlaying: false,
            player: new Audio(),
            songs: [
                {
                    title: "Rise",
                    artist: "League of legends",
                    src: require("./assets/music/Rise - League of legends.mp3"),
                    url: require('./assets/images/rise.jpg'),
                    time: "3:30",
                    isFavorite: false
                },
                {
                    title: "Warriors",
                    artist: "Imagine dragons",
                    src: require("./assets/music/Warriors - Imagine dragons.mp3"),
                    url: require('./assets/images/warriors.jpg'),
                    time: "2:50",
                    isFavorite: false
                },
                {
                    title: "Wildfire",
                    artist: "LEC-version",
                    src: require("./assets/music/Wildfire - LEC.mp3"),
                    url: require('./assets/images/wildfire.jpg'),
                    time: "3:28",
                    isFavorite: false
                },
            ]
        };
    },
    methods: {
        play(song) {
            if (typeof song.src != "undefined") {
                this.current = song;
                this.player.src = this.current.src;
            }
            this.player.play();
            this.player.addEventListener("ended", function () {
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
            this.player.pause();
            this.isPlaying = false;
        },
        next() {
            this.index++;
            if (this.index > this.songs.length - 1) {
                this.index = 0;
            }
            this.current = this.songs[this.index];
            this.play(this.current);
        },
        prev() {
            this.index--;
            if (this.index < 0) {
                this.index = this.songs.length - 1;
            }
            this.current = this.songs[this.index];
            this.play(this.current);
        },
        toggleFav(){
         this.songs[this.index]
          console.log('index:',this.songs[this.index])
        },
        created() {
            this.current = this.songs[this.index];
            this.player.src = this.current.src;
        }
    }
}
</script>

<style>
  *{
    margin: 0;
    padding: 0;
    box-sizing: border-box;
  }
  :root{
    background: #212121;
  }
  #app{
    font-family: sans-serif;
  }
  .image{
    border-radius: 50%;
    width: 100px;
    height: 100px;
  }
  .artist-image{
    width: 100px;
    height: 100px;
    border-radius: 10%;
  }
  .active{
    background-color:green;
  }
  header{
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 2em;
    background-color: #ffffff ;
    color: #212121
  }
  .fa-play,.fa-pause{
    font-size: 1.5em;
    color: #212121;
  }
  .fa-heart{
    font-size: 1.3em;
    color: #ffffff;
  }
  i{
    font-size: 1em;
    color: #ffffff;
  }
  main{
    width: 100%;
    max-width: 768px;
    margin:0 auto;
    padding: 2em;
  }
  .song-title, .current-title{
    color: #fff;
    font-size: 1.5em;
    font-weight: 700; 
    text-transform: capitalize;
    text-align: left;
  }
  .song-artist, .current-artist{
    color: #d1d1d1;
    font-size: 1em;
    font-weight: 500; 
    text-transform: capitalize;
    text-align: left;
    margin-bottom: 0.5em;
  }
  .player h2{
    text-align: left;
  }
  .blok{
    text-align: center;
    justify-content: center;
    display:flex;
  }
  .controls{
    display: flex;
    justify-content: center;
    align-items: center;
    padding: 30px 15px;
  }
  button{
    appearance: none;
    background: none;
    border: none;
    outline: none;
    cursor: pointer;
  }
    button:hover{
    opacity: 0.8;
  }
  .play, .pause{
    font-size: 1.5em;
    font-weight: 700;
    margin: 0 15px;
    padding: 15px 20px;
    border-radius: 50%;
    background-color: white;
  }
  .next, .prev{
    font-size: 1.2em;
    font-weight: 700;
    margin: 0 2em;
    color: #fff;
  }
  .playlist{
    margin-bottom: 1em;
    color: #fff
  }
  .playlist h3{
    font-size: 2em;
    color: #212121;
    font-weight: 600;
    margin-bottom: 30px;
    text-align: center;
  }
  .song{
    display: block;
    width: 100%;
    padding: 15px;
    font-size: 20px;
    font-weight: 700;
    cursor: pointer;
  }
  .song:hover{
    background-color: #212121;
  }
  .song.playing{
    color: #fff;
    background-image: linear-gradient(to right, #0000007c, #0000005c );
  }
</style>
