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
      <div v-if="tweets.number != 0">
        ユーザー {{ tweets.user }} | {{ tweets.text }} | {{ tweets.fav }}
        <button @click="Favorite(tweets.number)">ふぁぼ</button> |
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
      tweetss: [{ number: 0, user: "", text: "", fav: 0 }]
    };
  },
  methods: {
    Tweet() {
      axios.post("api/tweet", {
        tweet_text: this.tweet,
        user: this.user
      });
      this.tweet = "";
      this.user = "";
      setTimeout(this.reload(), 100);
    },
    reload() {
      axios.get("api/tweet").then(response => {
        console.log("response" + response.data);
        console.log("tweetss = " + this.tweetss);
        /*for (let index = 0; index < this.tweetss.length; index++) {
          this.tweetss[index].number = 0;
          this.tweetss[index].user = "";
          this.tweetss[index].text = "";
        }*/
        this.tweetss = [{ number: 0, user: "", text: "", fav: 0 }];
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
            text: response.data[index].tweet_text,
            fav: response.data[index].fav
          });
        }
      });
    },
    Delete(number) {
      axios.delete("api/tweet/" + number);
      setTimeout(this.reload(), 100);
    },
    Favorite(number) {
      axios.post("api/tweet/" + number);
      setTimeout(this.reload(), 100);
    }
  }
};
</script>
