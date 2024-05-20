<template>
     
    <div class="grid">
      <p v-if="error">Something went wrong...</p>
      <p v-if="loading">Loading...</p>
  
      <div v-else v-for="episode in filteredEpisodes" :key="episode.id" class="episode-card flex my-3  rounded-lg border-2 bg-slate-900">
         <img :src="episode.characters[1].image" alt="Episode Image" />
        <div class="episode-info">
          <h2 class="text-2xl text-gray-200 font-bold ">{{ episode.name }}</h2>
          <p>Episode: {{ episode.episode }}</p>
          <p>Air Date: {{ episode.air_date }}</p>
        </div>
      </div>
    </div>
  </template>
  <script >
  import gql from 'graphql-tag';
  import { useQuery } from '@vue/apollo-composable';
  
  const EPISODES_QUERY = gql`
    query Episodes {
      episodes{
        results {
          id
          name
          episode
          air_date
          characters {
            id
            image
            name
          }
        }
      }
    }
  `;
  
  export default {
    name: 'episodePage',
    setup() {
      const { result, loading, error } = useQuery(EPISODES_QUERY)
  
      return {
        result,
        loading,
        error,
      };
    }
 }
    
  
  </script>
    