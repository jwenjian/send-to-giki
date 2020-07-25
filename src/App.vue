<template>
  <div id="app">
    <div class="content-wrapper">
      <SendToGikiForm v-if="tokenExists" :token="token"></SendToGikiForm>
      <CreateBookmarkLink v-else></CreateBookmarkLink>
    </div>
    <div style="display: flex;justify-content: flex-end;padding-right: 2em;font-size: smaller;">
      <p>Made with ❤️ by <a href="https://github.com/jwenjian/send-to-giki">jwenjian/send-to-giki</a></p>
    </div>
  </div>
</template>

<script>
import SendToGikiForm from './components/SendToGikiForm'
import CreateBookmarkLink from './components/CreateBookmarkLink'

export default {
  name: 'App',
  components: {
    SendToGikiForm,
    CreateBookmarkLink
  },
  data() {
    return {
      token: '',
      tokenExists: false
    }
  },
  created() {
    const searchParams = new URLSearchParams(window.location.search);
    this.token = searchParams.get('token')
    this.tokenExists = searchParams.has('token')
  }
}
</script>

<style>
html body {
  margin: 0;
  padding: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  min-height: 100vh;
  background-image: linear-gradient(-225deg, #2CD8D5 0%, #C5C1FF 56%, #FFBAC3 100%);
}
.content-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
}
.content-wrapper > div {
  max-width: 60vh;
}
</style>
