<template>
  <!-- TODO: CSSで整える -->
  <div class="page mt-5 text-center">
    <form @submit.prevent="post" v-if="!isFound">
      <p><input type="text" v-model="name" placeholder="名前" name="name"/></p>
      <p><input type="password" v-model="password_digest" placeholder="パスワード" name="password_digest"/></p>
      <div class="login-btn">
        <button type="submit">確認</button>
      </div>
    </form>
    <Debt :member=member v-if="isFound/>
  </div>
</template>

<script>
import axios from 'axios'
import Debt from "@/components/Debt";

export default {
  components: {
    Debt
  },
  data () {
    return {
      isFound: false,
      name: "",
      password_digest: "",
      member: {}
    }
  },
  methods: {
  post() {
    const params = new URLSearchParams();
      params.append('name', this.name);
      params.append('password_digest', this.password_digest);
    this.$axios.$post("/api/v1/login", params)
      .then(function (response) {
        member = response
        isFound = true
        console.log(response);
      })
      .catch(function (error) {
        console.log(error);
      });
    }
 }
}
</script>
