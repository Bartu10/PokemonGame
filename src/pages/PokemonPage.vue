<template>
    <h1 v-if="!pokemon">Espere por favor...</h1>

    <div v-else>
        
        <div style="padding-right: 3%; text-align: right;">
            <img v-for="n in this.vidas" v-bind:key="n" style="width:20px" src="../assets/kisspng-love-heart-love-heart-romance-clip-art-picture-of-red-heart-5aaeb718420cb8.8640685015213995762706.png" />
        </div>
        <h1>Puntuacion: {{this.puntuacion}}</h1>
        <h1>¿Quién es este pokémon?</h1>
        
        <PokemonPicture 
            :pokemon-id="pokemon.id" 
            :show-pokemon="showPokemon" 
        />

        <PokemonOptions
            v-if="!showAnswer"
            :pokemons="pokemonArr"
            @selection-pokemon="checkAnswer"
        />

        <template v-if="showAnswer">
            <h2 class="fade-in">{{ message }}</h2>
            <button @click="newGame">
                Next
            </button>
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
            contador: 0,
            vidas: 3,
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
                this.contador += 1
                if(this.contador == 10){
                    if(this.vidas < 5){
                    this.vidas += 1                        
                    }
                    this.contador = 0
                }
            } else {
                this.message = `Oops, era ${ this.pokemon.name }`
                this.vidas -= 1
                if(this.vidas == 0){
                    this.reset()
                }
            }
        },
        newGame() {
            this.showPokemon = false
            this.showAnswer  = false
            this.pokemonArr  = []
            this.pokemon     = null
            this.mixPokemonArray()            

        }
,
        reset(){
            this.vidas = 3
            this.puntuacion = 0
        }
    },
    mounted() {
        this.mixPokemonArray()
    }

}
</script>
