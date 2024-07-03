<template>
  <div class="app">
    <header>
      <h1>The<strong>Anime</strong>Database</h1>
      <form class="search-box" @submit.prevent="HandleSearch">
        <input
          type="search"
          class="search-field"
          placeholder="Search for an anime..."
          required
          v-model="search_query"
        />
      </form>
    </header>
    <main>
      <div class="recommendCards" v-if="!hasSearched">
        <h2>Recommended Anime</h2>
        <div class="recommendations">
          <AnimeCard 
            v-for="anime in animeRecommend" 
            :key="anime.mal_id" 
            :anime="anime" 
          />
        </div>
      </div>
      <div class="cards" v-if="animeList.length > 0">
        <AnimeCard 
          v-for="anime in animeList" 
          :key="anime.mal_id" 
          :anime="anime" 
        />
      </div>
      <div class="no-results" v-else-if="noResults">
        <h2>Anime not found.</h2>
      </div>
    </main>
  </div>
</template>

<script>
import { ref, onMounted  } from "vue";
import AnimeCard from "./components/AnimeCard.vue";

export default {
  components: {
    AnimeCard
  },

  setup() {
    // Reactive variables
    const search_query = ref(""); // For storing the search query
    const animeList = ref([]); // For storing the search results
    const animeRecommend = ref([]); // For storing the recommended anime
    const noResults = ref(false); // For indicating if no search results were found
    const hasSearched = ref(false); // For indicating if a search has been made

    // Fetch recommended anime on component mount
    onMounted(async () => {
      try {
        const response = await fetch(`https://api.jikan.moe/v4/recommendations/anime?q=1`);
        const data = await response.json();
        animeRecommend.value = data.data.map(item => item.entry).flat() ;
        console.log("Anime Recommend")
        console.log(animeRecommend.value)
        
      } catch (error) {
        console.error('Error fetching data:', error);

      }
    });
      
    // Handle search functionality
    const HandleSearch = async () => {
      try {
        hasSearched.value = true;
        const response = await fetch(`https://api.jikan.moe/v4/anime?q=${search_query.value}`);
        const data = await response.json();
        animeList.value = data.data;
        noResults.value = data.data.length === 0; // Set noResults to true if no search results are found

        search_query.value = "";

      } catch (error) {
        console.error('Error fetching data:', error);
        noResults.value = true;

      }
    };

    // Return variables and methods to be used in the template
    return {
      search_query,
      animeList,
      noResults,
      hasSearched,
      animeRecommend, 
      HandleSearch
    };
  },
};
</script>

<style lang="scss">
* {
  background-color: #4EFFEF;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: "Fira Sans", sans-serif;
}

a {
  text-decoration: none;
}

header {
  padding-top: 50px;
  padding-bottom: 50px;

  h1 {
    color: #87849A;
    font-size: 42px;
    font-weight: 400;
    text-align: center;
    text-transform: uppercase;
    margin-bottom: 30px;

    strong {
      color: #313131;
    }
  }

  .search-box {
    display: flex;
    justify-content: center;
    padding-left: 30px;
    padding-right: 30px;

    .search-field {
      appearance: none;
      background: none;
      border: none;
      outline: none;
      background: #f3f3f3;
      box-shadow: 0px 4px 8px rgba(0, 0, 0, 0.15);
      display: block;
      width: 100%;
      max-width: 600px;
      padding: 15px;
      border-radius: 8px;
      color: #313131;
      font-size: 20px;
      transition: 0.4s;

      &::placeholder {
        color: #aaa;
      }

      &:focus,
      &:valid {
        color: #fff;
        background-color: #313131;
        box-shadow: 0px 0px 0px rgba(0, 0, 0, 0.15);
      }
    }
  }
}

main {
  max-width: 1200px;
  margin: 0 auto;
  padding-left: 30px;
  padding-right: 30px;

  h2 {
    color: #D8A7CA;
  }

  .cards {
    display: flex;
    flex-wrap: wrap;
    margin: 0 -8px;
  }

  .recommendations {
    display: flex;
    flex-wrap: wrap;
    margin: 0 -8px;
  }
}
</style>
