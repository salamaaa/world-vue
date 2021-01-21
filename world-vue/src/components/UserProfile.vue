<template>
  <div class="user-profile">
    <div class="user-profile__user-panel">
      <h1 class="user-profile__username">@{{ user.username }}</h1>
      <div class="user-profile__admin-badge" v-if="user.isAdmin">
        Admin
      </div>
      <div class="user-profile__admin-badge" v-else>
        User
      </div>
      <div class="user-profile__follower-count">
        <strong>Followers : {{ followers }}</strong>
      </div>
      <form class="user-profile__create-tweet" @submit.prevent="createNewTweet">
        <label for="newTweet"><strong>New Tweet</strong></label>
        <textarea id="newTweet"
                  rows="4"
                  v-model="newTweetContent"/>

        <div class="user-profile__create-tweet-type">
          <label for="tweetType"><strong>Type: </strong></label>
          <select id="tweetType" v-model="selectedTweetType">
            <option :value="item.value"
                    v-for="(item,index) in tweetTypes"
                    :key="index">
              {{ item.name }}
            </option>
          </select>
        </div>
        <button>Tweet</button>
      </form>

    </div>
    <div class="user-profile__tweets-wrapper">
      <TweetItem v-for="tweet in user.tweets"
                 :key="tweet.id"
                 :username="user.username"
                 :tweet="tweet"
                 @favourite="toggleFav"/>
    </div>
  </div>
</template>

<script>
import TweetItem from "./TweetItem";

export default {
  name: "UserProfile",
  components: {TweetItem},
  data() {
    return {
      newTweetContent: '',
      selectedTweetType: 'instant',

      tweetTypes: [
        {value: 'draft', name: 'Tweet Draft'},
        {value: 'instant', name: 'Tweet Instant'}
      ],
      followers: 0,
      user: {
        id: 1,
        username: '_John Doe',
        firstName: 'John',
        lastName: 'Doe',
        email: 'john@doe.net',
        isAdmin: false,
        tweets: [
          {id: 1, content: "Let's play football"},
          {id: 2, content: "Why don't we study"}
        ]
      }
    }
  },

  computed: {
    fullName() {
      return `${this.user.firstName} ${this.user.lastName}`;
    }
  },
  methods: {
    followUser() {
      this.followers++;
    },
    toggleFav(id) {
      console.log(`Favourite tweet # ${id}`);
    },
    createNewTweet(){
      if (this.newTweetContent && this.selectedTweetType !== 'draft'){
        this.user.tweets.unshift({
          id:this.user.tweets.length,
          content: this.newTweetContent
        })
      }
    }
  },

  //run whenever the component is loaded for the first time
  mounted() {
    this.followUser();
  },

  //run when data changed we use it to watch a change and handle it
  watch: {
    followers(newFollowerCount, oldFollowerCount) {
      if (oldFollowerCount < newFollowerCount) {
        console.log(`${this.user.username} has gained a follower!`);
      }
    }
  },

}
</script>

<style scoped>
.user-profile {
  display: grid;
  grid-template-columns: 1fr 3fr;
  width: 100%;
  padding: 50px 5%;
}

.user-profile__user-panel {
  display: flex;
  flex-direction: column;
  margin-right: 50px;
  padding: 20px;
  background-color: white;
  border-radius: 5px;
  border: 1px solid #dfe3e8;
}

h1 {
  margin-bottom: 0;
}

.user-profile__admin-badge {
  background-color: rebeccapurple;
  color: white;
  margin-right: auto;
  border-radius: 5px;
  padding: 0 10px;
  font-weight: bold;
}

.user-profile__tweets-wrapper {
  display: grid;
  grid-gap: 10px;
}

.user-profile__create-tweet {
  display: flex;
  flex-direction: column;
  padding-top: 20px;
  border-top: 1px solid #dfe3e8;
}

.user-profile__create-tweet-type {
  padding-top: 10px ;
  padding-bottom: 10px ;
}
</style>