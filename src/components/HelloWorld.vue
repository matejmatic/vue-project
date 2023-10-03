<template>
  <v-container>
    <v-main>
      
      <v-container>
        <div style="width: 40%; margin: auto;">
          <v-text-field
            v-model="search"
            placeholder="Search"
            prepend-inner-icon="mdi-magnify"
            filled
            dense
          ></v-text-field>
        </div>
        <v-row>
          <v-col
            v-for="pokemon in filteredPokemons"
            :key="pokemon.id"
            cols="4"
          >
            <v-card color="#4592CE">
              <v-img
                class="align-end"
                contain
                max-height="300"
                min-height="300"
                :src="pokemon.sprites.other.dream_world.front_default"
                gradient="to bottom, rgba(0,0,0,.1), rgba(0,0,0,.5)"
                cover 
                dark
              >
                <v-card-title class="text-h6 mb-1">{{ pokemon.name }}</v-card-title>
              </v-img>
              
              <v-card-subtitle class="pt-4" >
                <h2>Type:  {{ pokemon.types[0].type.name }}</h2>
              </v-card-subtitle>
              
            </v-card>

          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-container>
</template>

<script>

  import axios from 'axios'
  export default {
    name: 'HelloWorld',

    data: () => ({
      data: [],
      search: '',
    }),
    methods: {
      filteredPokemonsByType () {
      console.log("Filter")
      
      },
    },
    mounted () {
      axios.get('https://pokeapi.co/api/v2/pokemon?offset=0&limit=60')
        .then(res => {
          for (const i in res.data.results) {
            axios.get(res.data.results[i].url)
            .then(response => {
                this.data.push(response.data);
            })
          }
        })
        .catch(error => {
          console.log(error)
        })
    },
   
    computed: {
      filteredPokemons () {
        return this.data.filter((n) => {
          return n.name.match(this.search)
        });
      }
    }

  }
</script>
