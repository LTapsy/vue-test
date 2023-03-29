<template>
  <div class="container">
  <div class="phone-wrapper">
    <button class="big-logo" @click="getArtists" v-if="showArtistButton"><img src="./assets/images/lordify logo.png" alt=""></button>
    <div class="hero">
      <h1>
        Icons of the Era
      </h1>
      <p>Honoring the Best Artist of the 80's</p>
    </div>
    <div class="artist" v-if="hideArtist">
      <div v-for="p in artist" :key="p.id">
        <button class="artist-card" @click="getAlbums(p)" v-if="!p.clicked">
          <h3>
            {{ p.name}}
          </h3>
        </button>
      </div>
    </div>
    <div class="album" v-if="hideAlbum">
      <div class="artist-name">
        <h2>{{ artist[0].name }} Albums</h2>
      </div>
      <div v-for="p in albums" :key="p.id">
      <button class="album-card" @click="getSongs(p)"><h3>
        {{ p.name}}
        </h3>
      </button>
      </div>
    </div>
    <div class="Song" v-if="hideSong">
      <div class="album-name">
        <h2>Songs From The Album {{ albums[0].name }}</h2>
        <h5>Year Released : {{ albums[0].name }}</h5>
      </div>
      <ul>
      <li v-for="p in songs" :key="p.id">
        {{ p.track}}. <span>{{ p.name}}</span>
    </li>
    </ul>
    </div>
  </div>
  <div class="getStarted">
    <h1>Get Started</h1>
    <p>Here are some step-by-step instructions to help you get started with the music app:</p>
    <ol>
      <li>Open the music app by clicking on its icon on your device.</li>
      <li>Once the app loads, you will see a list of artists. Click on the artist that you want to explore.</li>
      <li>After selecting the artist, you will see a list of their albums. Click on the album that you are interested in.</li>
      <li>Once you have selected an album, the app will display a list of all the songs on that album.</li>
      <li>You can scroll through the list of songs using the scroll wheel on your device.</li>
    </ol>
    <p><i>Note: Please note that some songs or albums may not be available on the app depending on the API's data.</i></p>
  </div>
</div>
  


</template>

<script>
export default {
  data(){
    return{
      showArtistButton: true, // Add this property
      hideArtist: false,
      hideAlbum: false, // Add this property
      hideSong: false, // Add this property
      artist: [],
      albums: [],
      songs: []
    }
  },
  methods:{
    getArtists() {
      fetch('http://localhost:5000/artists')
        .then(response => response.json())
        .then(data => {
          this.artist = data.map(a => ({...a, clicked: false}))
          this.hideArtist = true; // Update the property
          this.showArtistButton = false; // Update the property
        })
    },
    getAlbums(x) {
      fetch('http://localhost:5000/albums')
        .then(response => response.json())
        .then(data => {
          this.albums = data.filter(albums => albums.artist_id == x.id).map(a => ({...a, clicked: false}));
          this.hideArtist = false;
          this.hideAlbum = true;
        });
      fetch('http://localhost:5000/artists')
        .then(response => response.json())
        .then(data => {
          this.artist = data.filter(artist => artist.id == x.id);
        })
    },
    getSongs(y) {
      fetch('http://localhost:5000/songs')
        .then(response => response.json())
        .then(data => {
          this.songs = data.filter(songs => songs.album_id == y.id);
          this.hideAlbum = false;
          this.hideSong = true;
        });
      fetch('http://localhost:5000/albums')
        .then(response => response.json())
        .then(data => {
          this.albums = data.filter(albums => albums.artist_id == y.id);
        });
    }
  }
}
</script>
