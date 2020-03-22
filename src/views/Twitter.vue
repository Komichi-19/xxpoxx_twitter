<template>
  <div class="Twitter">
    <div>
      <h1>xxpoitterxx</h1>
      <input type="text" v-model="user" />
      <input type="text" v-model="tweet" />
      <button @click="Tweet">tweet</button>
    </div>
    <div>
      <button @click="reload">reload</button>
    </div>
    <div>poweets</div>
    <div v-for="tweets in tweetss" :key="tweets.number">
      <div>
        ポイート番号{{ tweets.number }} | ユーザー {{ tweets.user }} |
        {{ tweets.text }} |
        <button @click="Delete(tweets.number)">Delete</button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "tweet",
  data() {
    return {
      tweet: "",
      user: "",
      tweetss: [{ number: 0, user: "hoge", text: "hoge" }]
    };
  },
  methods: {
    Tweet() {
      axios.post("api/tweet", {
        tweet_text: this.tweet,
        user: this.user
      });
    },
    reload() {
      axios.get("api/tweet").then(response => {
        console.log("response" + response.data);
        console.log("tweetss = " + this.tweetss);
        for (let index = response.data.length - 1; index >= 0; index--) {
          /*this.tweetss[index].number = 0;
          this.tweetss[index].user = "hoge";
          this.tweetss[index].text = "hoge";
          /*this.tweetss[index].number = response.data[index].number - 1;
          this.tweetss[index].user = response.data[index].user;
          this.tweetss[index].text = response.data[index].tweet_text;*/
          this.tweetss.push({
            number: response.data[index].number,
            user: response.data[index].user,
            text: response.data[index].tweet_text
          });
        }
      });
    },
    Delete(number) {
      axios.delete("api/tweet/" + number);
    }
  }
};
</script>
