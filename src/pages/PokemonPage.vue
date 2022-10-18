<template>
<h1 v-if="!pokemon">Espere por favor...</h1>
        <div v-else>
                <h1>¿Quién es este pokemón?</h1>
        <!-- TODO picture -->
        <PokemonPicture :pokemonId="pokemon.id" :showPokemon="showPokemon"/>
        <!-- TODO opciones -->
        <PokemonOptions :pokemons="pokemonArr"
        @selection="checkAnswer"
        />
        </div>

        <div v-if="showAnswer">
                <h2>{{mesagge}}</h2>
        <button @click="newGame">Nuevo Juego</button>
        </div>
        
 
</template>

<script>
import PokemonPicture from '@/components/PokemonPincture'
import PokemonOptions from '@/components/PokemonOptions'

import getPokemonOptions from '@/helpers/getPokemonOptions'

// console.log(getPokemonOptions())
export default {
        components:{PokemonPicture,PokemonOptions},
        data(){
            return{
                pokemonArr:[],
                pokemon:null,
                showPokemon:false,
                showAnswer:false,
                mesagge:''
         }
        },
        methods:{
              async  mixPokemonArray(){
                        this.pokemonArr = await getPokemonOptions()
                        const rnInt = Math.floor(Math.random() * 4)
                        this.pokemon = this.pokemonArr[rnInt]
                },
                checkAnswer(selectedId){
                        this.showPokemon = true
                        this.showAnswer = true
                        if(selectedId === this.pokemon.id){
                                this.mesagge = `Correcto, ${this.pokemon.name}`
                        }else{
                                this.mesagge = `Opss, era ${this.pokemon.name}`
                        }
                },
                newGame(){
                    this.showPokemon = false,
                    this.showAnswer = false,
                    this.pokemonArr = [],
                    this.mixPokemonArray(),
                    this.pokemon = null
                }
        },
        mounted(){
                this.mixPokemonArray(   )
        }
}
</script>

