<template>
  <form class="create-tweet-panel"
        @submit.prevent="createNewTweet"
        :class="{'--exceeded' : getCharacterCount > 180} ">

    <label for="newTweet"><strong>New Tweet</strong>
      ({{ getCharacterCount }}/180)
    </label>

    <textarea id="newTweet"
              rows="4"
              v-model="newTweetContent"/>

    <div class="create-tweet-panel__submit">
      <div class="create-tweet-type">
        <label for="tweetType"><strong>Type: </strong></label>

        <select id="tweetType" v-model="selectedTweetType">
          <option :value="item.value"
                  v-for="(item,index) in tweetTypes"
                  :key="index">
            {{ item.name }}
          </option>
        </select>

      </div>
      <button>Tweet it!</button>
    </div>
  </form>
</template>

<script>
export default {
  name: "CreateTweetPanel",
  data() {
    return {
      newTweetContent: '',
      selectedTweetType: 'instant',
      tweetTypes: [
        {value: 'draft', name: 'Tweet Draft'},
        {value: 'instant', name: 'Tweet Instant'}
      ]
    }
  },
  computed: {
    getCharacterCount() {
      return this.newTweetContent.length;
    }
  },
  methods: {
    createNewTweet() {
      if (this.newTweetContent && this.selectedTweetType !== 'draft') {
        this.$emit('add-tweet', this.newTweetContent)
      }
      this.newTweetContent = ''
    }
  }
}
</script>

<style lang="scss" scoped>
.create-tweet-panel {
  display: flex;
  flex-direction: column;
  margin-top: 20px;
  padding: 20px 0;

  textarea {
    border: 1px solid #dfe3e8;
    border-radius: 5px;
  }

  .create-tweet-panel__submit {

    display: flex;
    justify-content: space-between;

    .create-tweet-type {
      padding: 20px 0;

      select{
        cursor: pointer;
      }

    }

    button {
      padding: 5px 20px;
      margin: auto 0;
      border: none;
      border-radius: 5px;
      background-color: coral;
      color: white;
      font-weight: bold;
      cursor: pointer;
    }
  }

  &.--exceeded {
    color: red;
    border-color: red;

    .create-tweet-panel__submit {
      button {
        background-color: red;
        color: white;
      }
    }
  }
}
</style>