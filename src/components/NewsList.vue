<template>
<form @submit.prevent="searchNews" class="d-flex flexcolumn justify-content-center">
<div class="input-group mx-sm-3 mb-2">
 <label class="visually-hidden" for="search">Search</label>
 <input type="search" name="search" v-model="searchTerm"
id="search" class="form-control mb-2 mr-sm-2" placeholder="Enter search term here" />
 <button class="btn btn-primary mb-2">Search</button>
 </div>
 <p>You are searching for {{ searchTerm }} </p>
 </form>
    

    <ul class="news__list">
        <li v-for="article in articles" class="news__item"> 
        <img class = "images" :src = "article.urlToImage" alt = "News article image" />
            <h5> {{ article.title }}</h5>
            <p>{{ article.description }}</p>
         </li>
    </ul>
</template>

<script>
        export default{
            data() {
                return {
                    articles: [],
                    searchTerm: ' '
                };
            },
             created() {
                let self = this; 
                
                fetch('https://newsapi.org/v2/top-headlines?country=us',
                    {
                        headers: {
                            'Authorization': `Bearer ${import.meta.env.VITE_NEWSAPI_TOKEN}`
                         }
                    })
                        .then(function(response) {
                            return response.json();
                        })
                        .then(function(data) {
                            console.log(data);
                            self.articles = data.articles;
                        });
                    },
                    methods: {
                        searchNews() {
                            let self = this;
                            fetch('https://newsapi.org/v2/everything?q='+ self.searchTerm + '&language=en', {
                                headers: {
                                    'Authorization': `Bearer ${import.meta.env.VITE_NEWSAPI_TOKEN}`, }
                                })
                                .then(function(response) {
                                    return response.json();
                                    })
                                    .then(function(data) {
                                    console.log(data);
                                    self.articles = data.articles;
                                    });
                                    }
                                    }
    }
</script>

<style>
    img {
        width: 280px;
        height:200px;
    }
            
.news__list {
    padding-top: 20px;
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    list-style-type: none;
}
.news__item {
    border-top: 1px solid rgba(0, 0, 0, 1);
    border-bottom: 5px solid green;
    border-radius: 10px;
    box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 1);
    margin: 10px;
    padding: 10px;
    width: 320px;
}

</style>