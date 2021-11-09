<template>
    <h1 v-if="!pokemon">Espere por favor...</h1>
    <div v-else>
        <h1>¿Quién es este pokémon?</h1>
        <pokemon-picture :pokemonId="pokemon.id" :showPokemon="showPokemon" />
        <pokemon-options
            :pokemons="pokemonArr"
            @selection-pokemon="checkAnswer"
        />
        <template v-if="showAnswer" class="fade-in">
            <h2>{{ message }}</h2>
            <button @click="newGame">Nuevo Juego</button>
        </template>
    </div>
</template>

<script>
import PokemonOptions from '@/components/PokemonOptions.vue'
import PokemonPicture from '@/components/PokemonPicture.vue'
import getPokemonOptions from '@/helpers/getPokemonOptions'

export default {
    components: {
        PokemonOptions,
        PokemonPicture,
    },
    data() {
        return {
            pokemonArr: [],
            pokemon: null,
            showPokemon: false,
            showAnswer: false,
            message: '',
        }
    },
    methods: {
        async mixPokemonArr() {
            this.pokemonArr = await getPokemonOptions()
            const rndInt = Math.floor(Math.random() * 4)
            this.pokemon = this.pokemonArr[rndInt]
        },
        checkAnswer(pokemonId) {
            this.showPokemon = true
            this.showAnswer = true
            if (pokemonId == this.pokemon.id) {
                this.message = `Correcto, ${this.pokemon.name}`
            } else {
                this.message = `Oops era, ${this.pokemon.name}`
            }
        },
        newGame() {
            this.showAnswer = false
            this.showPokemon = false
            this.pokemonArr = []
            this.pokemon = null
            this.mixPokemonArr()
        }
    },
    mounted() {
        this.mixPokemonArr()
    },
}
</script>
