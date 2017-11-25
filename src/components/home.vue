<template>
  <div class="home">
    <h1>Tweets</h1>
    <div class="flex-cols actions">
      <div class="group">
        <label for="">Busca</label>
        <input type="text" class="search" v-model="search">
      </div>
      <div class="group">
        <label for="show-only-with-images">Mostras apenas os com imagens</label>
        <input type="checkbox" id="show-only-with-images" v-model="showOnlyWithImages">
      </div>
    </div>
    <div class="tweets">
      <div class="tweet" v-for="(tweet, index) in filteredTweets" :key="index">
        <div class="body">
          {{tweet.text}}
          <div class="medias">
            <div class="media" v-for="(media, index) in tweet.entities.media" :key="index">
              <img :src="media.media_url  " alt="">
            </div>
          </div>
        </div>
        <footer class="footer">
          <span class="username">
            <a :href="`https://www.twitter.com/${tweet.user.screen_name}`" target="_blank">
              @{{tweet.user.screen_name}}
            </a>
          </span>
          <div class="hashtags">
            <span class="hashtag" v-for="(hashtag, index) in tweet.entities.hashtags" :key="index">
              #{{hashtag.text}} &nbsp;
            </span>
          </div>
        </footer>
      </div>
    </div>
  </div>
</template>


<script>
// ADICIONE O PATH DO JSON COM UM ARRAY DE TWEETS
console.warn('Você precisa de um arquivo tweets.json em src/data com um arquivo json de um array com objetos de tweets')
const tweets = require('../data/tweets.json') || []

export default {
  name: 'home',
  data () {
    return {
      tweets: tweets,
      search: '',
      showOnlyWithImages: false
    }
  },
  computed: {
    filteredTweets () {
      let filteredResults = []
      if (this.search.length > 0) {
        filteredResults = this.tweets.filter((t) => t.text.toLowerCase().indexOf(this.search.toLowerCase()) > -1)
      } else {
        filteredResults = this.tweets
      }

      if (this.showOnlyWithImages) {
        filteredResults = filteredResults.filter((t) => t.entities.media && t.entities.media.length > 0)
      }

      return filteredResults
    }
  }
}
</script>

<style lang="scss" scoped>
.home {
}

.actions {
  margin: 20px;
}

.tweet {
  border: 1px solid grey;
  padding: 20px 15px;
  margin-bottom: 10px;
  display: flex;
  flex-direction: column;
  .body {
    display: flex;
    margin-bottom: 5px;
  }
  .footer {
    display: flex;
    justify-content: space-between;
    margin-top: 30px;
  }

  .username {
  }

  .media {
    img {
      width: 200px;
      height: 200px;
    }
  }
}
</style>
