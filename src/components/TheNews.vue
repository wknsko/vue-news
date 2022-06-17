<template>
  <div class="container">
    <div class="searchbar">
      <form @submit.prevent>
        <input type="text" v-model="searchWord" placeholder="search...." />
      </form>
      <div class="search-icons">
        <i class="fas fa-search" @click="newSearch()"></i>
        <i class="fas fa-times" @click="closeSearch()"></i>
      </div>
    </div>

    <h1>{{title}}</h1>

    <div class="result-list">
      <article v-for="(article, index) in articles" :key="index">
        <header>
          <img v-if="article.media" v-lazy="article.media" />
          <i v-else class="fas fa-image"></i>
        </header>
        <section>
          <h2>{{ article.title }}</h2>
          <p class="date">{{ article.published_date }}</p>
        </section>
        <footer>
          <a :href="article.link" target="_blank"><i class="fas fa-chevron-right"></i></a>
        </footer>
      </article>

      <div class="page">
        <i class="fas fa-chevron-left" @click="prevPage()"></i>
        <i class="fas fa-chevron-right" @click="nextPage()"></i>
      </div>


    </div>
  </div>
</template>

<script>
export default {
  props:['apiKey'],
  data() {
    return {
      page: 1,
      articles: [],
      searchWord: '',
      title: ''
      
    }
  },
  methods: {
    async fetchNews() {
      var url = new Request('https://api.newscatcherapi.com/v2/latest_headlines?countries=US&when=12h&lang=en&page_size=5&page=' + this.page);
      const res = await fetch(url, {
        method: 'GET',
        headers: {
          "x-rapidapi-host": "newscatcher.p.rapidapi.com",
          "x-api-key": this.apiKey
        },
      });
      let data = await res.json();
      this.articles = data.articles;
      console.log(data)
      this.title = 'Top News'
      
    }, async fetchSearchNews() { 
      var url = new Request('https://api.newscatcherapi.com/v2/search?q=' + this.searchWord +'&countries=US&lang=en&page_size=5&page=' + this.page);
      const res = await fetch(url, {
        method: 'GET',
        headers: {
          "x-rapidapi-host": "newscatcher.p.rapidapi.com",
          "x-api-key": this.apiKey
        },
      });
      let data = await res.json();
      this.articles = data.articles;
      console.log(data)
      this.title = 'Searched News'
    },
nextPage(){
  this.page++
  if(this.searchWord === '') {
  
  this.fetchNews() 
  } else {
    this.fetchSearchNews()
  }
}, 
    prevPage() {
      this.page--
      if (this.searchWord === '') {

        this.fetchNews()
      } else {
        this.fetchSearchNews()
      }
    }, 
    closeSearch(){
      this.page = 1;
      this.fetchNews();
      this.searchWord= '';
    },
  newSearch() {
    this.page = 1;
    this.fetchSearchNews()
  }
  },
  created() {
    this.fetchNews()
  }
}
</script>

<style lang="scss">
.container {
  position: relative;
}

.search {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  width: 100%;
  height: 40px;
  font-size: 1rem;
}

.page {
  display: flex;
  justify-content: center;
  align-items: center;
  padding: 10px 0px;

  i {
    font-size: 1.4rem;
    cursor: pointer;
    color: #9e9e9e;
    padding: 0 50px;
  }
}


input {
  padding: 0 100px 0 20px;
  margin: 0;
  width: calc(100% - 120px);
  height: 40px;
  border: none;
  font-size: 1rem;
  color: #fff;
  background-color: #222;

  &:focus {
    outline: none;
  }
}

.search-icons {
  position: absolute;
  right: 20px;
  top: 10px;
  color: #fff;

  i {
    margin-left: 15px;
    cursor: pointer;
  }

}

.date {
  position: relative;
  margin-bottom: auto
}

h1 {
  font-size: 2rem;
  text-transform: uppercase;
  color: #7e3b3b;
  padding: 0 20px;
}

h2 {
  font-size: 18px;
  font-weight: 400;
}

.result-list {
  padding-top: 2px;
}

article {
  display: grid;
  grid-template-columns: 200px auto 40px;
  grid-template-rows: 180px;
  border-bottom: 1px solid #ccc;
  overflow: hidden;



  header {
    display: flex;
    justify-content: center;
    align-items: center;
    overflow: hidden;

    img {
      max-width: 100%;
      height: auto;
      padding-left: 5px;
    }

    i {
      font-size: 2rem;
    }
  }

  section {
    margin: 0;
    padding: 0px 10px;
    height: 100px;
    width: 100%;




  }

  footer {
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 1.6rem;
    color: #888;
    cursor: pointer;

    i {
      color: #9e9e9e;

      &:visited {
        color: #7e3b3b
      }
    }

  }
}
</style>