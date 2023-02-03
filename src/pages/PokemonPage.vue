<template>
    <h1 v-if="!pokemon">Espere por favor...</h1>

    <div v-else>
        <h2>Puntuacion: {{this.puntuacion}}</h2>
        <h1>¿Quién es este pokémon?</h1>
        
        <PokemonPicture 
            :pokemon-id="pokemon.id" 
            :show-pokemon="showPokemon" 
        />

        <PokemonOptions 
            :pokemons="pokemonArr"
            @selection-pokemon="checkAnswer"
        />

        <template v-if="showAnswer">
            <h2 class="fade-in">{{ message }}</h2>
            {{ setTimeout(newGame(),5) }}
        </template>

    </div>
    
</template>

<script>
import PokemonOptions from '@/components/PokemonOptions.vue'
import PokemonPicture from '@/components/PokemonPicture.vue'

import getPokemonOptions from '@/helpers/getPokemonOptions'


export default {
    components: { PokemonOptions, PokemonPicture },
    data() {
        return {
            puntuacion: 0,
            pokemonArr: [],
            pokemon: null,
            showPokemon: false,
            showAnswer: false,
            message: ''
        }
    },
    methods: {
        async mixPokemonArray() {
            this.pokemonArr = await getPokemonOptions()

            const rndInt = Math.floor( Math.random() * 4 )
            this.pokemon = this.pokemonArr[ rndInt ]
        },
        checkAnswer( selectedId ) {
            this.showPokemon = true
            this.showAnswer  = true

            if( selectedId === this.pokemon.id ) {
                this.message = `Correcto, ${ this.pokemon.name }`
                this.puntuacion += 1
            } else {
                this.message = `Oops, era ${ this.pokemon.name }`
                this.puntuacion = 0
            }
        },
        newGame() {
            
            this.showPokemon = false
            this.showAnswer  = false
            this.pokemonArr  = []
            this.pokemon     = null
            this.mixPokemonArray()            

        }
    },
    mounted() {
        this.mixPokemonArray()
    }

}
</script>
