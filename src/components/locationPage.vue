<template>
  <div class="mt-4 flex justify-center my-10">
    <div class=" m-2">
         <select name="" id="" class="text-center   rounded-md text-gray-700 font-bold p-2">
          <option value="" class="text-sm font-bold hover:bg-slate-900" selected>Episode...</option>
          <option value="" class="text-sm font-bold hover:bg-blue-500">episode-1</option>
          <option value="" class="text-sm font-bold hover:bg-slate-900">episode-2</option>
          <option value="" class="text-sm font-bold hover:bg-slate-900">episode-3</option>
          <option value="" class="text-sm font-bold hover:bg-slate-900">episode-4</option>
          <option value="" class="text-sm font-bold hover:bg-slate-900">episode-5</option>
          <option value="" class="text-sm font-bold hover:bg-slate-900">episode-6</option>
          <option value="" class="text-sm font-bold hover:bg-slate-900">episode-7</option>
           <option value="" class="text-sm font-bold hover:bg-blue-500">episode-1</option>
          <option value="" class="text-sm font-bold hover:bg-slate-900">episode-2</option>
          <option value="" class="text-sm font-bold hover:bg-slate-900">episode-3</option>
          <option value="" class="text-sm font-bold hover:bg-slate-900">episode-4</option>
          <option value="" class="text-sm font-bold hover:bg-slate-900">episode-5</option>
          <option value="" class="text-sm font-bold hover:bg-slate-900">episode-6</option>
           <option value="" class="text-sm font-bold hover:bg-blue-500">episode-1</option>
          <option value="" class="text-sm font-bold hover:bg-slate-900">episode-2</option>
          <option value="" class="text-sm font-bold hover:bg-slate-900">episode-3</option>
          <option value="" class="text-sm font-bold hover:bg-slate-900">episode-4</option>
          <option value="" class="text-sm font-bold hover:bg-slate-900">episode-5</option>
          <option value="" class="text-sm font-bold hover:bg-slate-900">episode-6</option>
           <option value="" class="text-sm font-bold hover:bg-blue-500">episode-1</option>
          <option value="" class="text-sm font-bold hover:bg-slate-900">episode-2</option>
          <option value="" class="text-sm font-bold hover:bg-slate-900">episode-3</option>
          <option value="" class="text-sm font-bold hover:bg-slate-900">episode-4</option>
          <option value="" class="text-sm font-bold hover:bg-slate-900">episode-5</option>
          <option value="" class="text-sm font-bold hover:bg-slate-900">episode-6</option>
          
        </select>  
    </div>
    <input
      type="text"
      v-model="search"
      name=""
      id=""
      class="border-2 w-96 px-3  mr-2 border-slate-500 rounded-lg text-md text-gray-800 font-bold"
      placeholder="Search for episode"
    />

    <button class="bg-blue-500 text-white px-3 py-2 text-xl rounded-lg">search</button>
  </div>
<div class="flex gap-2 ">
 
  <div class="w-full">
    
  <div class="grid grid-cols-1 mx-12">
    <p v-if="error">Something went wrong...</p>
    <p v-if="loading">Loading...</p>

    <div
      v-else
      v-for="(episode,index) in filteredEpisodes"
      :key="episode.id"
      class=""
    >
       <div class="episode-info text-3xl  font-bold my-3 rounded-lg border-2  flex-row bg-white m-auto ">
        <h1 class="text-3xl text-center text-red-500">{{episode.episode}}</h1>
        <h1 class=" font-bold text-gray-500">Name :   {{ episode.name }}</h1>
        <p class=" text-gray-500 ">Episode : <span class="">{{ episode.episode }}</span></p>
        <p class="text-gray-500">Air Date: {{ episode.air_date }}</p>
        <p class="text-gray-500">created: {{ episode.created }}</p>

      </div>
      
       <div>
        <h1 class="text-3xl font-bold text-blue-400">
          Characters List -- <span class="text-red-300">{{ episode.episode }}</span>
        </h1>
        <div class="grid md:grid-cols-2 lg:grid-cols-3">
          <div  v-for="c in episode.characters" :key="c.id" class="p-10">  
            <!--Card 1-->
            <div class="max-w-sm rounded overflow-hidden shadow-lg bg-gray-900 rounded-2xl">
              <div class="relative">
                <div 
                :class="{
                  'bg-green-500': c.status === 'Alive',
                  'bg-red-500': c.status === 'Dead',
                  'bg-blue-700': c.status === 'unknown'

                }"
                class="inline-block  rounded-lg px-3 py-0 text-lg font-bold  mr-2 mb-2 absolute right-2 top-2 text-white">{{ c.status}}</div>

              <img class="w-full h-64 object-cover" :src="c.image" alt="Mountain">
              </div>
              <div class="px-6 py-4">
                <div class="font-bold text-xl mb-2"> {{c.name}}</div>
                 
              </div>

              <div class="px-6 pt-4 pb-2">
                <span class="inline-block bg-gray-200 rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mr-2 mb-2">{{ c.status }}</span>

                <span class="inline-block bg-gray-200 rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mr-2 mb-2">{{ c.species }}</span>
                <span class="inline-block bg-gray-200 rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mr-2 mb-2">{{ c.gender }}</span>
              </div>
            </div>
          </div>
        </div>
       </div>
      
    </div>
  </div>
  </div>
</div>
</template>
<script>
import gql from 'graphql-tag'
import { useQuery } from '@vue/apollo-composable'

const EPISODES_QUERY = gql`
  query Episodes {
    episodes {
      results {
        id
        name
        episode
        air_date
        created
        characters {
          id
          name
          status
          gender
          species
          image
          
        }
      }
    }
  }
`

export default {
  name: 'App',
  setup() {
    const { result, loading, error } = useQuery(EPISODES_QUERY)

    return {
      result,
      loading,
      error
    }
  },
  data() {
    return {
      search: ''
    }
  },
  computed: {
    filteredEpisodes() {
      return this.result.episodes.results.filter((eps) => {
        return eps.name.toLowerCase().includes(this.search.toLowerCase())
      })
    }
  }
}
</script>

<style scoped>
.episode-card {
  display: flex;
  border: 1px solid #ddd;
  border-radius: 5px;
}

.episode-card img {
  width: 120px;
  height: 180px;
  object-fit: cover;
  border-top-left-radius: 5px;
  border-bottom-left-radius: 5px;
}

.episode-info {
  padding: 15px;
}

.episode-info h2 {
  font-size: 18px;
  margin-bottom: 5px;
}
</style>
