<template>
  <v-app>
    <v-container>
      <div class="row mx-0 d-flex justify-center">
        <div
          class="col-4"
          v-for="pokemon in visiblePokemons"
          :key="getId(pokemon)"
        >
          <div class="pokedex">
            <div class="cartoes-pokemon">
              <div class="cartao-pokemon aberto">
                <div class="cartao-topo">
                  <div class="detalhes">
                    <h2 class="nome">{{ pokemon.name }}</h2>
                    <span> {{ getId(pokemon) }} </span>
                  </div>

                  <div class="cartao-imagem">
                    <img
                      :src="`https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/${getId(
                        pokemon
                      )}.png`"
                      :alt="pokemon.name"
                      width="70%"
                    />
                  </div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
      <div class="d-flex justify-center">
        <button type="button" class="btn btn-outline-secondary" @click="showMore()">Mostrar Mais</button>
      </div>
    </v-container>
  </v-app>
</template>

<script>
import axios from "axios";

export default {
  name: "App",

  components: {},

  data() {
    return {
      pokemons: [],
      visiblePokemons: [],
    };
  },

  methods: {
    getNames() {
    axios
    .get("https://pokeapi.co/api/v2/pokemon?limit=493")
    .then((response) => {
      this.pokemons = response.data.results.map((pokemon) => {
        pokemon.name =
          pokemon.name.charAt(0).toUpperCase() + pokemon.name.slice(1);
        return pokemon;
      });
      this.visiblePokemons = this.pokemons.slice(0, 20);
    })
    .catch((error) => {
      console.log(error);
    });
},


    getId(pokemon) {
      return Number(pokemon.url.split("/")[6]);
    },
 

  showMore() {
      this.visiblePokemons = this.pokemons.slice(0, this.visiblePokemons.length + 20);
    },
  },

  mounted() {
    this.getNames();
    this.showMore();
  }
}
</script>

<style>
#app {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #dadfe8;
  -webkit-background-size: cover;
  -moz-background-size: cover;
  -o-background-size: cover;
  background-size: cover !important;
  background-position: center;
  min-height: 100vh;
}

.cartao-pokemon {
  display: none;
  box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;
  background-color: rgb(245, 52, 52);
  border-radius: 10px;
}

.cartao-pokemon .cartao-topo {
  padding: 30px 40px 0; /*em cima, lado e embaixo*/
}

.cartao-pokemon .nome {
  margin-bottom: 5px;
}

.cartao-pokemon .tipo {
  font-size: 12px;
  background-color: #ffffff;
  opacity: 0.7;
  border-radius: 10px;
  padding: 2px 10px;
  box-shadow: rgba(100, 100, 111, 0.2) 0px 7px 29px 0px;
}

.cartao-pokemon .detalhes {
  color: #ffffff;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.cartao-pokemon .cartao-imagem {
  align-items: center;
}

.cartao-pokemon img {
  max-height: 100%;
}

.cartao-pokemon.aberto {
  display: block;
}
</style>
