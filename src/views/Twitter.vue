<template>
  <div class="Twitter">
    <div>
      <h1>xxpoitterxx</h1>
      ユーザー <input type="text" v-model="user" /> ポイート
      <input type="text" v-model="tweet" />
      <button @click="Tweet">tweet</button>
    </div>
    <div>
      <button @click="reload">reload</button>
    </div>
    <div>poweets</div>
    <div class="poweets">
      <button @click="back_page()">BACK PAGE</button>
      <button @click="next_page()">NEXT PAGE</button>
      <table border="1" width="100%" class="sample">
        <tr>
          <th bgcolor="#000000"><font color="#FFFFFF">ユーザー</font></th>
          <th bgcolor="#000000"><font color="#FFFFFF">ポイート</font></th>
          <th bgcolor="#000000"><font color="#FFFFFF">ふぁぼ</font></th>
          <th bgcolor="#000000"><font color="#FFFFFF">削除</font></th>
        </tr>
        <tr v-for="tweets in tweetss" :key="tweets.number">
          <th class="users" v-if="tweets.number != 0">{{ tweets.user }}</th>
          <th class="poweet" v-if="tweets.number != 0">
            {{ tweets.text }}
          </th>
          <th class="tenpa-" v-if="tweets.number != 0">
            {{ tweets.fav }}
            <button @click="Favorite(tweets.number)">ふぁぼ</button>
          </th>
          <th class="tenpa-" v-if="tweets.number != 0">
            <button @click="Delete(tweets.number)">Delete</button>
          </th>
        </tr>
      </table>
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
      page: 1,
      hoge: 9,
      tweetss: [{ number: 0, user: "", text: "", fav: 0 }]
    };
  },
  methods: {
    Tweet() {
      axios
        .post("api/tweet", {
          tweet_text: this.tweet,
          user: this.user
        })
        // eslint-disable-next-line no-unused-vars
        .then(res => this.reload());
      this.tweet = "";
      this.user = "";
    },
    reload() {
      axios.get("api/tweet/page/" + this.page).then(response => {
        console.log("response" + response.data);
        console.log("tweetss = " + this.tweetss);
        /*for (let index = 0; index < this.tweetss.length; index++) {
          this.tweetss[index].number = 0;
          this.tweetss[index].user = "";
          this.tweetss[index].text = "";
        }*/
        if (this.hoge >= response.data.length - 1) {
          this.hoge = response.data.length - 1;
        } else {
          this.hoge = 9;
        }
        console.log(this.hoge);
        console.log(this.page);
        this.tweetss = [{ number: 0, user: "", text: "", fav: 0 }];
        for (let index = this.hoge; index >= 0; index--) {
          //response.data.length - 1
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
      // eslint-disable-next-line no-unused-vars
      axios.delete("api/tweet/" + number).then(res => this.reload());
    },
    Favorite(number) {
      // eslint-disable-next-line no-unused-vars
      axios.post("api/tweet/" + number).then(res => this.reload());
    },
    next_page() {
      this.page += 10;
      this.reload();
    },
    back_page() {
      this.page -= 10;
      this.reload();
    }
  }
  /*<div v-for="tweets in tweetss" :key="tweets.number">
        <div v-if="tweets.number != 0">
          ユーザー {{ tweets.user }} | {{ tweets.text }} | {{ tweets.fav }}
          <button @click="Favorite(tweets.number)">ふぁぼ</button> |
          <button @click="Delete(tweets.number)">Delete</button>
        </div>
      </div>
  */
};
</script>

<style>
body {
  background: #a79e71;
  font-family: Meiryo;
}
div {
  background: #f1f1f1;
  margin: 10px auto;
}
.poweets {
  background: #b38080;
}
.poweet {
  background: #abb380;
}
.tenpa- {
  width: 10%;
}
.users {
  background: #f0f0ea;
  width: 20%;
  word-break: break-all;
}
table.sample {
  border-collapse: collapse;
}
</style>
