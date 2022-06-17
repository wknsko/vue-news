<template>
  <the-news :apiKey="apiKey"></the-news>
  
</template>

<script>

import TheNews from './components/TheNews.vue'


export default {
  name: 'App',
  data() {
    return {
      apiKeyOld: '314cd1ec7d024aa189ede126021964e2',
      apiKey: 'EXsPHEtpq4yyRxC8Rzib4L_453bWUSHzccjOo0Pk3v8'
    }
  },
  components: {
    
    TheNews
  }
}
</script>

<style>
@import url('https://use.fontawesome.com/releases/v5.7.2/css/all.css');
@import url('https://fonts.googleapis.com/css2?family=Heebo:wght@300&display=swap');

body {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  background: linear-gradient(to bottom, #c7d1ca, #8faba7);
}

#app {
  font-family: Helvetica, sans-serif;
  font-size: 1rem;
  color: #333;
  width: 100%;
  max-width: 480px;
  height: 95%;
  max-height: 1000px;
  background-color: #fff;
  box-shadow: 2px 5px 10px rgba(5, 5, 5, 0.222); 
  overflow-y: scroll;
  overflow-x: hidden;

}

::-webkit-scrollbar {
  display: none;
}
</style>
