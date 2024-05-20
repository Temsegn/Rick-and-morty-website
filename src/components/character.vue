<template>
  
 <div class=" border-2 m-2 border-gray-900 rounded-2xl">
  
<div class="flex flex-wrap ">
  
  <p v-if="error">Something went wrong...</p>
  <p v-if="loading">Loading...</p>
  <div
    v-else
    v-for="character in result.characters.results"
    :key="character.id"
    class="p-2"
  >
    <!--Card 1-->
    <div class="w-32 h-32 rounded-md overflow-hidden shadow-lg bg-gray-900">
         
      <img class="w-full h-20 object-cover" :src="character.image" alt="Mountain">
       <div class="px-2 pt-1">
        <div class=" text-sm">{{ character.name }}</div>
       </div>
      
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
  }
}
 
  </script>