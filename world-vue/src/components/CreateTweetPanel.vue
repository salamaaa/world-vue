<template>
  <form class="create-tweet-panel"
        @submit.prevent="createNewTweet"
        :class="{'--exceeded' : newTweetCharacterCount > 180} ">

    <label for="newTweet"><strong>New Tweet</strong>
      ({{ newTweetCharacterCount }}/180)
    </label>

    <textarea id="newTweet"
              rows="4"
              v-model="state.newTweetContent"/>

    <div class="create-tweet-panel__submit">
      <div class="create-tweet-type">
        <label for="tweetType"><strong>Type: </strong></label>

        <select id="tweetType" v-model="state.selectedTweetType">
          <option :value="item.value"
                  v-for="(item,index) in state.tweetTypes"
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
import {reactive, computed} from 'vue';

export default {
  name: "CreateTweetPanel",


  setup(props,context) {
    const state = reactive({
      newTweetContent: '',
      selectedTweetType: 'instant',
      tweetTypes: [
        {value: 'draft', name: 'Tweet Draft'},
        {value: 'instant', name: 'Tweet Instant'}
      ]
    })

    const newTweetCharacterCount = computed(
        () => state.newTweetContent.length);

    function createNewTweet() {
      if (state.newTweetContent && state.selectedTweetType !== 'draft') {
        context.emit('add-tweet', state.newTweetContent);
      }
      state.newTweetContent = '';
    }

    return {state, newTweetCharacterCount,createNewTweet};
  },
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

      select {
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