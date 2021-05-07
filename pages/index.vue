<template>
 <div class="page mt-5 text-center">
    <form @submit.prevent="post">
      <p><input type="text" v-model="name" placeholder="名前" name="name"/></p>
      <p><input type="password" v-model="password_digest" placeholder="パスワード" name="password_digest"/></p>
      <div class="login-btn">
        <button type="submit">確認</button>
      </div>
    </form>
 </div>
</template>

<script>
import axios from 'axios'
export default {
 data () {
   return {
     name: "",
     password_digest: ""
   }
 },
 methods: {
  post() {
    const params = new URLSearchParams();
      params.append('name', this.name);
      params.append('password_digest', this.password_digest);
    axios.post("http://localhost:8000/api/v1/login", params)
      .then(function (response) {
        console.log(response);
      })
      .catch(function (error) {
        console.log(error);
      });
  }
 }
}
</script>