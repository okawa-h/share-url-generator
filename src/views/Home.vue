<template lang="pug">
  .home
    .inner
      p.title Share URL Generator
      section.section#input
        h2.subtitle Input
        .input-item
          label URL
          input(type="text" v-model="input.url" @change="onChange")
        .input-item
          label Via #[small (for Twitter)]
          input(type="text" v-model="input.via" @change="onChange")
        .input-item
          label Text #[small (for Twitter)]
          textarea(rows="5" v-model="input.text" @change="onChange")
        .input-item
          label Related #[small (for Twitter)]
          input(type="text" v-model="input.related" @change="onChange")
        .input-item
          label Hashtags #[small (for Twitter)]
          input(type="text" v-model="input.hashtags" @change="onChange")
      section.section#result
        h2.subtitle Result
        ul.link-list
          li(v-for="(value, name) in shareList" :class="name")
            label {{ value }}
            input(type="text" :value="encodeData[name]" @click="onClickInput")
            template(v-if="encodeData[name]")
              a(:href="encodeData[name]" target="_blank" rel="noopener") Link
              button(@click="witeToClipboard") Copy
</template>

<script>
export default {
  name: "home",
  data() {
    return {
      input: {
        url: "",
        text: "",
        via: "",
        related: "",
        hashtags: ""
      },
      encodeData: {
        facebook: "",
        twitter: "",
        line: ""
      }
    };
  },
  computed: {
    shareList: () => {
      return { twitter: "Twitter", facebook: "facebook", line: "LINE" };
    }
  },
  methods: {
    onChange() {
      const url = encodeURIComponent(this.input.url);
      this.setEncodeTwitter(url);
      this.setEncodeFacebook(url);
      this.setEncodeLINE(url);
    },
    onClickInput(event) {
      event.target.select();
    },
    witeToClipboard(event) {
      const text = event.target.previousElementSibling.previousElementSibling.value;
      navigator.clipboard
        .writeText(text)
        .then(() => alert("Copy"))
        .catch(e => alert(e));
    },
    setEncodeTwitter(url) {
      this.encodeData.twitter = `https://twitter.com/intent/tweet?url=${url}`;
      const list = ["text", "via", "related", "hashtags"];
      for (const value of list) {
        if (this.input[value]) {
          const encode = encodeURIComponent(this.input[value]);
          this.encodeData.twitter += `&${value}=${encode}`;
        }
      }
    },
    setEncodeFacebook(url) {
      this.encodeData.facebook = `https://www.facebook.com/sharer/sharer.php?u=${url}`;
    },
    setEncodeLINE(url) {
      this.encodeData.line = `https://social-plugins.line.me/lineit/share?url=${url}`;
    }
  }
};
</script>

<style scoped lang="scss">
.home {
  width: 100%;
  padding: 60px 0;
  font-family: "Press Start 2P", cursive;
  .inner {
    width: 780px;
    margin: 0 auto;
    .section {
      + .section {
        margin-top: 60px;
      }
    }
  }
}
.title {
  margin-bottom: 30px;
  font-size: 28px;
}
.subtitle {
  margin-bottom: 15px;
  font-size: 22px;
}
#input {
  .input-item {
    display: flex;
    align-items: flex-start;
    + .input-item {
      margin-top: 10px;
    }
    label {
      width: 150px;
      padding-top: 5px;
      font-size: 14px;
      small {
        display: block;
        margin-top: 5px;
        font-size: 0.6em;
      }
    }
    textarea,
    input {
      flex-grow: 1;
      padding: 0.4em 0.5em;
      border: 2px solid #ddd;
      border-radius: 3px;
      font-size: 12px;
      transition: border-color 0.15s ease-in-out;
      &:focus {
        border-color: #000;
      }
    }
  }
}
#result {
  .link-list {
    li {
      display: flex;
      align-items: center;
      + li {
        margin-top: 10px;
      }
      label {
        width: 150px;
        font-size: 14px;
      }
      input {
        flex-grow: 1;
        padding: 0.4em 0.5em;
        border: 2px solid #ddd;
        border-radius: 3px;
        font-size: 12px;
        transition: border-color 0.15s ease-in-out;
        &:focus {
          border-color: #000;
        }
      }
      a {
        margin-left: 15px;
        color: #000;
        font-size: 12px;
      }
      button {
        margin-left: 15px;
        color: #000;
      }
      &.facebook {
        color: #1877f2;
      }
      &.twitter {
        color: #55acee;
      }
      &.line {
        color: #00c300;
      }
    }
  }
}
</style>
