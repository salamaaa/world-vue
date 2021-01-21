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
</style>