<template>
    <nav class="">
      <h1 class="block text-3xl font-bold text-gray-700">
        <a href="" class="no-underline text-gray-200"> <span class="text-blue-500">Rick</span> and Morty Wiki </a>
      </h1>
    </nav>
    <div class="mt-4 flex">
      <input
        type="text"
        v-model="search"
        name=""
        id=""
        class="border-2 w-96 px-3 py-2 mr-2 border-slate-500 rounded-sm text-xl text-gray-800 font-bold"
        placeholder="Type episode name"
      />
      <button class="bg-blue-500 text-white px-3 py-2 text-xl rounded-sm">search</button>
    </div>
  
   
  
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
  <script>
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
    name: 'App',
    setup() {
      const { result, loading, error } = useQuery(EPISODES_QUERY)
  
      return {
        result,
        loading,
        error,
      };
    },
    data()
    {
      return{
        search:''
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
    width: 150px;
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
  
  