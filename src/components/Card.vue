<template>
<div class="container-pokemon">
    <div class="container-search">
        <input 
            class="search" 
            type="text" 
            placeholder="Buscar Pokemon..." 
            v-model="namePokemon"
            >
    </div> 
    <div class="container-pokedex">  
        <article class="pokemon" v-for=" pokemon in PokemonFilter" :key="pokemon.id">
            <figure>
                <img class="img-pokemon" 
                    :src="pokemon.sprites.other.dream_world.front_default" 
                    :alt="pokemon.name"
                >
            </figure>
            <section>
                <h2 class="pokeName"> {{ pokemon.name }} </h2>
                <h3 class="pokeType"> 
                    {{ pokemon.types.map((typeInfo) => typeInfo.type.name).join(" | ") }} 
                </h3>
            </section> 
        </article>
    </div>
</div>         
</template>

<script>

import axios from 'axios';

export default {
 name: "Card",
    data(){
        return{
            url: "https://pokeapi.co/api/v2/pokemon?offset=0&limit=151%22",
            pokemons:[],
            namePokemon:""
        }
    },
    methods:{

    async getPokemon() {
        for (let index = 1; index <= 151; index++){
            await axios.get(`https://pokeapi.co/api/v2/pokemon/${index}`)
                        .then(({data}) => {
                            this.pokemons.push(data)
                        })
                        .catch(error => console.log(error))
        }
    },

    },
    mounted(){
        this.getPokemon()
        this.getImgType()
    },
    computed:{
       PokemonFilter(){
            let opPokemon = [];

            opPokemon = this.pokemons.filter((pokemon) => {
                return(
                    pokemon.name.toLowerCase().indexOf(this.namePokemon.toLowerCase()) > - 1
                );   
            });
                 
            return opPokemon
            
        } 
    }
}
</script>

<style scoped>

.container-pokedex{
    display: flex;
    flex-wrap: wrap;
    align-items: space-between;
    justify-content: center;
    margin: 0 auto;
    text-align: center;
}

.pokemon{
    background-color: #eee;
    border-radius: 20px;
    box-shadow: 0 3px 15px rgb(100 100 100 / 50%);
    margin: 10px;
    padding: 20px;
    text-align: center;
    list-style: none;
}

.img-pokemon{
    width: 120px;
    height: 120px;
    text-align: center;

}

.pokeName{
    margin: 15px 0 7px;
    letter-spacing: 1px;
    font-family: "Flexo-Demi",arial,sans-serif;
    color: #313131;
    text-transform: none;
    font-size: 145%;
    margin-bottom: 5px;
}

.pokeName:first-letter{
    text-transform: uppercase
}

.pokeType{
    background-color: rgba(0, 0, 0, 0.1);
    border-radius: 10px;
    font-size: 0.8em;
    padding: 5px 10px;
}

.container-search{
  display:flex;
  justify-content: center;
  align-items: center;
  margin-top: 1rem;
  flex-direction: row;
  flex-wrap: nowrap 
}

.search{
  display: flex;
  justify-content: center;
  align-items: center;
  width: 20rem;
  height: 2rem;
  border: 3px solid #FF494C;
  border-radius: 5px;
  outline: none;
  color: rgb(29, 29, 29);
  text-align: center
}

.search:focus{
  color: black;
  font-size: 12px;
  font-weight: bold;
}

.searchButton {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 2rem;
  height: 2rem;
  margin-left: 0.2rem;
  border: 3px solid #fc1519;
  background: #fc1519;
  text-align: center;
  color: #fff;
  border-radius:5px;
  cursor: pointer;
  font-size: 20px;
}

</style>