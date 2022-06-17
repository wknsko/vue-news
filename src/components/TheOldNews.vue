<template>
    <div class="container">
        <div class="searchbar">
            <form @submit.prevent>
                <input type="text" v-model="searchWord" placeholder="search...." />
            </form>
            <div class="search-icons">
                <i class="fas fa-search" @click="fetchSearchNews()"></i>
                <i class="fas fa-times" @click="closeSearch()"></i>
            </div>
        </div>

        <h1>{{title}}</h1>


        <div class="result-list">
            <article v-for="(article, index) in articles" :key="index">
                <header>
                    <img v-if="article.urlToImage" v-lazy="article.urlToImage" />
                    <i v-else class="fas fa-image"></i>
                </header>
                <section>
                    <h2>{{ article.title }}</h2>
                    <p class="date">{{article.publishedAt.slice(0, article.publishedAt.indexOf('T'))}}</p>
                </section>
                <footer>
                    <a :href="article.url" target="_blank"><i class="fas fa-chevron-right"></i></a>
                </footer>
            </article>
            <div class="page">
                <i class="fas fa-chevron-left" @click="prevPage()"></i>
                <div class="current-page">{{shownPage}}</div>
                <i class="fas fa-chevron-right" @click="nextPage()"></i>
            </div>

        </div>


    </div>
</template>

<script>


export default {
    props: ['apiKey'],
    data() {
        return {
            apiUrl: '',
            articles: [],
            searchWord: '',
            currentPage: 1,
            isEnd: false,
            shownPage: 1,
            title: ''

        }
    },
    methods: {
        async fetchSearchNews() {
            let req = new Request('https://newsapi.org/v2/everything?q=' + this.searchWord + '&pageSize=5&page=' + this.currentPage + '&apiKey=' + this.apiKey);
            const response = await fetch(req);
            if (!response.ok) {
                this.isEnd = true;
                const message = `An error has occured: ${response.status}`;
                throw new Error(message);

            }
            const data = await response.json();
            if (data.articles.length !== 0) {
                this.articles = data.articles;
                this.shownPage = this.currentPage
                this.isEnd = false;
            } else {
                this.isEnd = true;
                this.shownPage = this.currentPage - 1;
            }
            this.title = 'Searched headlines'

        },
        async fetchData() {
            let req = new Request('https://newsapi.org/v2/top-headlines?country=us&pageSize=5&page=' + this.currentPage + '&apiKey=' + this.apiKey);
            const response = await fetch(req);
            if (!response.ok) {
                const message = `An error has occured: ${response.status}`;
                throw new Error(message);
            }
            const data = await response.json();


            this.searchWord = '';
            if (data.articles.length !== 0) {
                this.articles = data.articles;
                this.isEnd = false;
            } else {
                this.isEnd = true;
                this.shownPage = this.currentPage - 1;
            }
            this.title = 'Top headlines'

        }, nextPage() {
            this.currentPage++;
            if (this.searchWord.length !== 0) {
                this.fetchSearchNews();

            } else {
                this.fetchData();
                this.shownPage++
            }


        }, prevPage() {
            if (this.currentPage !== 1) {
                this.currentPage--;

                if (this.searchWord.length !== 0) {
                    this.fetchSearchNews();
                    this.shownPage--
                } else {
                    this.fetchData();
                    this.shownPage--

                }
            }
        }, closeSearch() {
            this.currentPage = this.shownPage = 1;
            this.fetchData();
        }
    },
    created() {
        this.fetchData();
    },
    components: {

    }
}
</script>

<style lang="scss" scoped>
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
        color: #9e9e9e
    }
}

.current-page {
    padding: 0 20px;
    font-size: 1.4rem;
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