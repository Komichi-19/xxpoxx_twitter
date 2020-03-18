<template>
  <div class="Twitter">
    <div>
      <h1>xxpoitterxx</h1>
      <input type="text" v-model="user" />
      <input type="text" v-model="tweet" />
      <button @click="Tweet">tweet</button>
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
      user: ""
    };
  },
  methods: {
    Tweet() {
      axios.post("/api/tweet", {
        tweet: this.tweet,
        user: this.user
      });
    }
  }
};
</script>
