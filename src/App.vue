<template>
  <div id="app">
    <pm-header />

    <div class="section">
      <nav class="nav has-shadow">
        <div class="container">
          <input
            type="text"
            class="input is-large"
            placeholder="Search"
            v-model="searchQuery"
          >
          <a
            class="button is-info is-large"
            @click="search"
          >
            Search songs
          </a>
          <a class="button is-danger is-large">&times;</a>
        </div>
      </nav>

      <div class="container results">
         <div class="columns is-multiline tracks">
           <div
            class="column"
            v-for="(track, index) in tracks"
            :key="index"
          >
            <pm-track :track="track" />
          </div>
         </div>

         <span>{{ searchMessage }}</span>
      </div>
    </div>

    <pm-footer />
  </div>
</template>


<script>
import trackService from './services/tracks';
import PmFooter from './components/layout/Footer.vue';
import PmHeader from './components/layout/Header.vue';
import PmTrack from './components/Track.vue';

export default {
  name: 'app',
  components: {
    PmHeader,
    PmFooter,
    PmTrack
  },
  data() {
    return {
      searchQuery: '',
      tracks: []
    }
  },
  methods: {
    search() {
      if (!this.searchQuery) {
        return;
      }
      trackService.search(this.searchQuery)
        .then((data) => {
          this.tracks = data.tracks.items;
        });
    }
  },
  computed: {
    searchMessage() {
      return `${this.tracks.length} song found`;
    }
  }
}
</script>


<style lang="scss">
  @import './styles.scss';

  .results {
    margin-top: 50px;
  }

  .tracks {
    // display: grid;
    // grid-template-columns: repeat(auto-fill, minmax(500px, 1fr));
    // grid-template-rows: repeat(auto-fill, minmax(200px, 1fr));
    // overflow-y: auto;
  }
</style>
