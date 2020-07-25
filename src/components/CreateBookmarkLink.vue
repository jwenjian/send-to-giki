<template>
  <div>
    <h3>Create your own bookmark link 🚀</h3>
    <p>
      Input your secret token into below input area then
      <strong>drag and drop</strong> the link to your bookmark 🔖
    </p>
    <p>Once you are done, go to your favirate page and click the bookmark, a popup window will appear, just click [send] button and the link will be send to your giki.</p>
    <p>Enjoy！ 🎉</p>
    <textarea
      v-model="token"
      name="token"
      id="token"
      cols="40"
      rows="10"
      placeholder="Put your token here..."
    ></textarea>
    <p>Your token is just in your bookmark, your browser, nowhere else</p>
    <h4>Your bookmark link :</h4>
    <a title="Drag and drop this link to bookmark" id="link" :href="bookmarkLink">Send to Giki</a>
  </div>
</template>

<script>
export default {
  name: "create-bookmark-link",
  data() {
    return {
      token: "",
    };
  },
  computed: {
    bookmarkLink() {
      if (this.token && this.token.length > 0) {
        return encodeURI(
          `javascript: (function (win, name, desc) { win.open('https://send-to-giki.netlify.app' + '?source=' + win.location.host + '&token=${this.token}' + '&url=' + encodeURIComponent(win.location.href) + (name ? '&name=' + encodeURIComponent(name) : '') + (desc ? '&desc=' + encodeURIComponent(desc) : ''), 'Send to giki', 'location=' + 'no' + ',toolbar=' + 'no' + ',width=600,height=600,left=' + (win.screenX + (win.outerWidth - 500) / 2) + ',top=' + (win.screenY + (win.outerHeight - 740) / 2)) })(window, document.title, getSelection ? getSelection().toString() : '')`
        );
      } else {
        // if no token, make sure the link opened will be directed to the create bookmark link page
        return encodeURI(
          `javascript: (function (win, name, desc) { win.open('https://send-to-giki.netlify.app' + '?source=' + win.location.host + '&url=' + encodeURIComponent(win.location.href) + (name ? '&name=' + encodeURIComponent(name) : '') + (desc ? '&desc=' + encodeURIComponent(desc) : ''), 'Send to giki', 'location=' + 'no' + ',toolbar=' + 'no' + ',width=600,height=600,left=' + (win.screenX + (win.outerWidth - 500) / 2) + ',top=' + (win.screenY + (win.outerHeight - 740) / 2)) })(window, document.title, getSelection ? getSelection().toString() : '')`
        );
      }
    },
  },
};
</script>

<style>
#link {
  cursor: grab;
}
</style>