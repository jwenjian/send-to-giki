<template>
  <div>
    <h3>Send to Giki 🚀</h3>
    <div v-show="!sending">
      <p>{{ title }}</p>
      <p>{{ url }}</p>
      <p>Description</p>
      <textarea v-model="desc" cols="40" rows="5" placeholder="Input description here(markdown is recommended)..."></textarea>
      <button @click="send">Send</button>
    </div>
    <div v-show="sending">
      <div v-show="!finished">
        <h4>Sending...</h4>
        <p>❕Do not close current window ❕</p>
      </div>
      <div v-show="finished">
        <div v-show="success">
          <p>👌 Success！ 😊</p>
          <p>
            <a href @click="closeWindow">Okay</a>
          </p>
        </div>
        <div v-show="!success">
          <p>❌ Failed！ 😭</p>
          <p>
            <a href @click="toRetry">Back To Retry</a>
          </p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import API from '@gikiapp/sdk'

export default {
  name: "send-to-giki-form",
  props: {
    token: {
      required: true,
    },
  },
  data() {
    return {
      title: "",
      desc: "",
      source: "",
      url: "",
      sending: false,
      finished: false,
      success: false,
    };
  },
  methods: {
    toRetry() {
      this.sending = false;
      this.finised = false;
      this.success = false;
    },
    closeWindow() {
      window.close();
    },
    send() {
      this.sending = true;
      let text = `\n${this.title}\n\n${this.desc}\n\n${this.url}\n`;
      let payload = {
        text: text,
        actions: [],
        tags: ["bookmark", "send-to-giki"],
      };
      console.log(payload);
      const api = new API({ env: 'production', token: this.token})
      api.save("talks", payload)
        .then((resp) => {
          if (resp.ok) {
            this.finished = true;
            this.success = true;
            console.log(resp);
          } else {
            this.finished = true;
            this.success = false;
            console.error(`send to giki.app failed: ${resp.status}`)
          }
        })
        .catch((err) => {
          this.finished = true;
          this.success = false;
          console.error(err);
        });
    },
  },
  created() {
    const searchParams = new URLSearchParams(window.location.search);
    this.title = searchParams.get("name");
    this.desc = searchParams.get("desc");
    this.source = searchParams.get("source");
    this.url = decodeURIComponent(searchParams.get("url"));
    console.log(this.token);
  },
};
</script>

<style>
</style>
