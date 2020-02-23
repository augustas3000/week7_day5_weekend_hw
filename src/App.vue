<template lang="html">

  <div class="app-main-container">

    <header>
      <h1>HEADER</h1>
    </header>

    <div class="map-and-sidebar-container">
      <div class="side-bar">
        <div class="form-goup">
            <div class="">
                <input v-model="munroSearchString" type="text" name="" value="" placeholder="Search munros">
            </div>
        </div>

      </div>

        <!-- make munros render on the map based on search bar results -->
        <div class="map-container">

          <my-map v-if="munroSearchString" v-bind:munros="munrosFiltered"></my-map>
          <my-map v-else v-bind:munros="munros"></my-map>
        </div>

    </div>


  </div>





</template>

<script>
// import { eventBus } from "@/main.js";
// import VuedogBeerList from "@/components/VuedogBeerList";
// import VuedogBeerInfo from "@/components/VuedogBeerInfo";
// import VuedogHeader from "@/components/VuedogHeader";
// import VuedogFavouritesList from "@/components/VuedogFavouritesList";
import MyMap from './components/MyMap.vue'

export default {
  name: "app",
  components: {
    "my-map": MyMap
  },
  data() {
    return {
      munros: [],
      munroSearchString: ""
    };
  },
  computed: {
    munrosFiltered() {

      if (this.munroSearchString) {
        return this.munros.filter(munro => {
          return munro.name.toLowerCase().includes(this.munroSearchString.toLowerCase())
        })
      }
    }
  },
  methods: {
      getMunros: function() {
      fetch('https://munroapi.herokuapp.com/munros').then(res => res.json()).then(munros => this.munros = munros)
      }
  },

  mounted() {
    this.getMunros();
  }

}
</script>

<style lang="css" scoped>


  .map-and-sidebar-container {
    display: flex;
    flex-grow: 1;
    flex-flow: row;
  }

  .side-bar {
    width: 20%;
  }

  .map-container {
    width: 80%;
  }

</style>
