<template>
    <v-container class="grey lighten-3">
        <v-row>
          <v-col
            cols="12"
            sm="3"
          >
            <v-sheet
              rounded="lg"
              min-height="368"
            >
              <!--  -->
              <v-list color="transparent">
                <v-list-item
                  v-for="n in data"
                  :key="n.id"
                  link
                >
                  <v-list-item-content>
                    <v-list-item-title>
                        <div class="row">
                           <div class="col-md-6"><h3>{{ n.name }}</h3></div>
                           <div class="col-md-6"><v-btn color="primary" v-on:click="getName(n.name)">Show</v-btn></div> 
                        </div>
                            
                    </v-list-item-title>
                  </v-list-item-content>
                </v-list-item>

                <v-divider class="my-2"></v-divider>

                <v-list-item
                  
                  color="grey lighten-4"
                >
                  <v-list-item-content>
                    <v-list-item-title>
                        <v-btn color="teal" v-if="showPrev" icon small @click="prevPage">
                            <v-icon>mdi-chevron-left</v-icon>
                          </v-btn>
                        
                        <v-btn color="teal" icon small @click="nextPage">
                          <v-icon>mdi-chevron-right</v-icon>
                        </v-btn>
                    </v-list-item-title>
                  </v-list-item-content>
                </v-list-item>
              </v-list>
            </v-sheet>
          </v-col>

          <v-col
            cols="12"
            sm="6"
          >

            <v-card v-if="this.pokemon" class="mx-auto" max-width="600" color="#4592CA" :elevation="24" style="border: 1px solid;">
                <v-img cover height="100%" width="400" class="mx-auto white--text align-end" :src="this.pokemon.sprites.other.dream_world.front_default"></v-img>
                <v-card-title>Name: {{ pokemon.name }}</v-card-title>
                
                <v-card-text class="text--primary">
                  <v-list rounded>
                    <v-list-item-group
                      
                    >
                     <div class="row">
                      <div class="col-md-4"><h2>Type: {{ pokemon.types[0].type.name }}</h2></div>
                      <div class="col-md-4"><h2>Height: {{ pokemon.weight }}</h2></div>
                      <div class="col-md-4"><h2>Weight: {{ pokemon.height }}</h2></div>
                     </div>
                     
                    </v-list-item-group>
                  </v-list>

                  </v-card-text>

                <v-card-actions class="justify-start">
                
                </v-card-actions>
            </v-card>

            <v-sheet v-else
              min-height="70vh"
              rounded="lg"
            >
              <!--  -->
                <img src="https://logos-world.net/wp-content/uploads/2020/05/Pokemon-Logo.png" width="100%" />
            </v-sheet>
            
          </v-col>

          <v-col
            cols="12"
            sm="3"
          >
            <v-sheet
              rounded="lg"
              min-height="268"
              
            >
              <!--  -->
                <v-form class="px-3">
                  <v-text-field label="Name" v-model="name" :rules="nameRules"></v-text-field>
                  <v-text-field label="Type" v-model="type">Type</v-text-field>
                  <v-text-field label="Weight" v-model="weight" type="number">Weight</v-text-field>
                  <v-text-field label="Height" v-model="height" type="number">Height</v-text-field>
                  <v-btn class="success mx-0 my-3" @click="createPokemon">Add pokemon</v-btn>
                </v-form>
            </v-sheet>
          </v-col>
        </v-row>
      </v-container>
  
</template>

<script>

import axios from 'axios'

export default {
    name: 'PokemonComponent',
    data: () => ({
        data: [],
        pokemon: '',
        error: [],
        name: '',
        type: '',
        weight: '',
        height: '',
        lastname: '',
        showPrev: false,
        currentUrl: 'https://pokeapi.co/api/v2/pokemon?offset=0&limit=10',
    }),
    methods: {
        createPokemon () {
          if (this.name && this.type && this.weight && this.height) {
            alert("Uspjesno ste dodali novog pokemona")
            this.name = ''
            this.type = ''
            this.height = '',
            this.weight = ''
          } else {
            alert("Error")
          }
            
        },
        getName (name) {
            axios.get('https://pokeapi.co/api/v2/pokemon/' + name)
            .then(res => {
                this.pokemon = res.data
            })
            .catch(error => {
                console.log(error)
            })
        },
        prevPage () {
        
        axios.get(this.prevUrl)
          .then(res => {
              this.prevUrl = res.data.previous
              this.data = res.data.results
             
              if (res.data.previous === null) {
                this.showPrev = false
                this.currentUrl = 'https://pokeapi.co/api/v2/pokemon?offset=0&limit=10'
              }
          })
          .catch(error => {
            console.log(error)
          }) 
      },
  
      nextPage () {
        this.showPrev = true
        axios.get(this.nextUrl)
          .then(res => {
              this.data = res.data.results
              this.nextUrl = res.data.next
              this.prevUrl = res.data.previous
          })
          .catch(error => {
            console.log(error)
          })

      },
    },
    mounted () {
        axios.get(this.currentUrl)
          .then(res => {
                this.next = res.data
                this.data = res.data.results
                this.nextUrl = res.data.next
                this.results = res.data.results
                console.log(this.results)
          })
          .catch(error => {
            console.log(error)
          })
      }
  }
</script>