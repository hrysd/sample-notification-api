<template>
  <div id="app" class="container">
    <div class="row">
      <h1>Notification API サンプル</h1>

      <template v-if="browserStatus === 'available'">
        <p v-if="enable">通知が有効</p>
        <form @submit.prevent="notify">
          <input type="text" class="u-full-width" v-model="title" placeholder="タイトルを入力">
          <textarea class="u-full-width" v-model="body" placeholder="中身を入力"/>

          <button
            type="submit"
            class="button-primary"
            :disabled="body.length < 1 || title.length < 1"
          >通知する</button>
        </form>
      </template>
      <template v-else>
        <a
          href="https://developer.mozilla.org/en-US/docs/Web/API/Notifications_API/Using_the_Notifications_API#Browser_compatibility"
          target="_blank"
          rel="noopener"
        >ブラウザの対応状況を確認してください</a>
      </template>
    </div>
  </div>
</template>

<script>
export default {
  name: "app",
  data() {
    return {
      browserStatus: "",
      enable: false,
      title: "",
      body: ""
    };
  },
  mounted() {
    if ("Notification" in window) {
      this.browserStatus = "available";
    } else {
      this.browserStatus = "unavailable";
    }

    Notification.requestPermission(permission => {
      if (permission === "granted") {
        this.enable = true;
      }
    });
  },
  methods: {
    notify() {
      if (Notification.permission === "granted") {
        const n = new Notification(this.title, { body: this.body });
        console.log(n);
      } else {
        alert("通知を有効にしてください");
      }
    }
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
