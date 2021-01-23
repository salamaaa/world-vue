<template>
  <div class="user-profile">
    <div class="user-profile__sidebar">
      <div class="user-profile__user-panel">
        <h1 class="user-profile__username">@{{ state.users[userId - 1].username }}</h1>
        <div class="user-profile__admin-badge" v-if="state.users[userId - 1].isAdmin">
          Admin
        </div>
        <div class="user-profile__admin-badge" v-else>
          User
        </div>
        <div class="user-profile__follower-count">
          <strong>Followers : {{ state.followers }}</strong>
        </div>
        <CreateTweetPanel @add-tweet="addTweet"/>
      </div>
    </div>
    <div class="user-profile__tweets-wrapper">
      <TweetItem v-for="tweet in state.users[userId - 1].tweets"
                 :key="tweet.id"
                 :username="state.users[userId - 1].username"
                 :tweet="tweet"
      />
    </div>
  </div>
</template>

<script>
import TweetItem from "../components/TweetItem";
import CreateTweetPanel from "../components/CreateTweetPanel";
import {reactive, computed} from 'vue';
import {useRoute} from 'vue-router';

export default {
  name: "UserProfile",
  components: {TweetItem, CreateTweetPanel},

  setup() {
    //declare the route using useRoute from vue-router
    const route = useRoute();

    const state = reactive({
      followers: 0,
      users: [{
        id: 1,
        username: 'Muhamed Salama',
        firstName: 'Muhamed',
        lastName: 'Salama',
        email: 'mosala@yahoo.com',
        isAdmin: true,
        tweets: [
          {id: 1, content: "Let's talk more"},
          {id: 2, content: "Let's chat online"}
        ]
      },
        {
          id: 2,
          username: "Muhamed Gaber",
          firstName: "Muhamed",
          lastName: "Gaber",
          email: "mogab@yahoo.com",
          isAdmin: false,
          tweets: [
            {id: 1, content: "Let's have fun"},
            {id: 2, content: "Let's walk outside"}
          ]
        },
        {
          id: 3,
          username: "Kamal Salama",
          firstName: "Kamal",
          lastName: "Gaber",
          email: "belgab@yahoo.com",
          isAdmin: false,
          tweets: [
            {id: 1, content: "Let's play video games"},
            {id: 2, content: "Let's hangout"}
          ]
        }
      ]

    });

    //get the userId paramter which i write in the link and assign it to the
    //userId var through computed part
    const userId = computed(() =>
        route.params.userId
    );

    function addTweet(tweet) {
      state.user.tweets.unshift({
        id: state.user.tweets.length + 1,
        content: tweet
      });
    }

    return {state, addTweet, userId};
  },
}
</script>

<style lang="scss" scoped>
.user-profile {
  display: grid;
  grid-template-columns: 1fr 3fr;
  grid-gap: 50px;
  padding: 50px 5%;

  .user-profile__user-panel {
    display: flex;
    flex-direction: column;
    margin-bottom: auto;
    padding: 20px;
    background-color: white;
    border-radius: 5px;
    border: 1px solid #dfe3e8;

    h1 {
      margin: 0;
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
      margin-bottom: auto;
    }
  }
}
</style>