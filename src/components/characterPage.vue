<template>
  <div class="mt-4 flex justify-center">
    <input
      type="text"
      v-model="search"
      name=""
      id=""
      class="border-2 w-96 px-3 py-2 mr-2 border-slate-500 rounded-sm  "
      placeholder="Search for character "
    />
    <button class="bg-blue-500 text-white px-3 py-2 text-xl rounded-sm">search</button>
  </div>
   

  <div class="">
    <a href="" class="block text-2xl font-bold text-gray-700">Clear Filter</a>
    <div id="app" class="container mx-auto bg-white border-grey-200 border mt-20">
      <div v-for="(item, index) in mydatas" :key="index">
        <div class="tab__header grid grid-cols-1">
          <a
            href="#"
            class="tab__link mb-1 p-4 bg-indigo-500 hover:bg-indigo-600 no-underline text-white flex justify-between rounded"
            @click.prevent="item.active = !item.active"
          >
            <strong> {{ item.name }}</strong>
            <span class="text-gray-200" v-show="!item.active">&#10133;</span>
            <span class="text-gray-200" v-show="item.active">&#10134;</span>
          </a>
        </div>
        <div class="tab__content p-2" v-show="item.active">
          <slot />
          <div v-if="index === 0" class="flex gap-3">
            <button type="button" class="bg-gray-300 px-3 py-1 rounded-lg">Alive</button>
            <button type="button" class="bg-gray-300 px-3 py-1 rounded-lg">Dead</button>
            <button type="button" class="bg-gray-300 px-3 py-1 rounded-lg">Unknown</button>
          </div>
          <div v-if="index === 1" class="flex gap-3">
            <button type="button" class="bg-gray-300 px-3 py-1 rounded-lg">Male</button>
            <button type="button" class="bg-gray-300 px-3 py-1 rounded-lg">Female</button>
          </div>
          <div v-if="index === 2" class="flex gap-3">
            <button type="button" class="bg-gray-300 px-3 py-1 rounded-lg">Human</button>
            <button type="button" class="bg-gray-300 px-3 py-1 rounded-lg">Non-human</button>
          </div>
        </div>
      </div>
    </div>
  </div>
<div class="grid  grid-cols-1">
  
  <p v-if="error">Something went wrong...</p>
  <p v-if="loading">Loading...</p>
  <div
    v-else
    v-for="character in filteredCharacters"
    :key="character.id"
    class="p-8"
  >
    <!--Card 1-->
    <div class="max-w-sm rounded-3xl overflow-hidden shadow-lg bg-gray-900">
      <div class="relative">
        <div 
        :class="{
          'bg-green-500': character.status === 'Alive',
          'bg-red-500': character.status === 'Dead',
          'bg-blue-700': character.status === 'unknown'

        }"
        class="inline-block  rounded-lg px-3 py-0 text-lg font-bold  mr-2 mb-2 absolute right-2 top-2 text-white">{{ character.status}}</div>

      <img class="w-full h-64 object-cover" :src="character.image" alt="Mountain">
      </div>
      <div class="px-6 py-4">
        <div class="font-bold text-xl">{{ character.name }}</div>
        <p class="text-gray-700 text-base"></p>
      </div>
      <div class="px-6 pt-4 pb-2 flex-row">
         
        <span
        class="block bg-gray-200 rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mr-2 mb-2"
        >Gender : {{ character.gender }}</span
      >
      <span
      class="block bg-gray-200 rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mr-2 mb-2"
      >Species : {{ character.species }}</span
    >
    <span
    class="block bg-gray-200 rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mr-2 mb-2"
    >Last known location : {{ character.location.name }}</span
  >
      </div>
     
    </div>
    <h1 class="text-blue-500 text-3xl font-bold mt-6">
      Episode List <small class="text-gray-500 text-sm"> ({{character.name}} participate)</small>
   </h1>
   <div class="grid md:grid-cols-2 lg:grid-cols-3 gap-4">
    <div  v-for="episode in character.episode" :key="episode.id" class="border-2 rounded-lg flex-row">  
      
      <p class="text-3xl text-center my-4 font-bold text-white ">
           {{ episode.name }}
      </p>
      <p class="text-2xl ml-12 ">
        episode :{{ episode.episode }}
   </p>
      <p class="text-2xl ml-12 ">
        Air date : {{ episode.air_date }}
     </p>
     <p class="text-2xl ml-12 ">
      Created : {{ episode.created }}
   </p>

        
    </div>
  </div>
  </div>
</div>
</template>

<script >
import gql from 'graphql-tag'
import { useQuery } from '@vue/apollo-composable'

const CHARACTERS_QUERY = gql`
  query Characters {
    characters {
      results {
        id
        name
        image
        status
        species
        gender
        location {
          id
          name
        }
        episode {
          id
          name
          air_date
          episode
          created

        }
      }
    }
  }
`

export default {
  name: 'CharacterPage',
  setup() {
    const { result, loading, error } = useQuery(CHARACTERS_QUERY)
    return {
      result,
      loading,
      error
    }
  },

  data() {
    return {
      mydatas: [
        { id: 1, name: 'Status', active: false, status: ['Alive', 'Dead', 'Unknown'] },
        { id: 2, name: 'Gender', active: false, gender: ['Male', 'Female'] },
        { id: 3, name: 'Species', active: false, species: ['mom', 'hhh'] }
      ],
      search: '',
     }
  },
  computed: {
    filteredCharacters() {
      return this.result.characters.results.filter((res) => {
        return res.name.toLowerCase().includes(this.search.toLowerCase())
      })
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;

   
  margin-top: 60px;
}

</style>
