<template>
  <div class="card">
    <div class="card-image">
      <figure class="image is-4by4">
        <img :src="user.avatar_url" alt="Profile Picture" />
      </figure>
    </div>
    <div class="card-content">
      <p class="title is-4"><a :href="user.web_url">{{ user.name }}</a></p>
      <p class="subtitle is-6">{{ user.username }}</p>
      <div class="content">
        {{ user.bio }}
      </div>
    </div>
    <footer class="card-footer">
      <p class="card-footer-item">
        <span>{{ user.location }}</span>
      </p>
      <p class="card-footer-item">
        <span><a :href="user.website_url">{{ user.website_url_nice }}</a></span>
      </p>
    </footer>
  </div>
</template>

<script>
export default {
  name: "GitlabUser",
  props: {
    gitlabToken: {
      type: String,
      required: true,
    }
  },
  data() {
    return {
      user: {}
    };
  },
  created() {
    fetch("https://gitlab.com/api/v4/user", {
      headers: {
        "PRIVATE-TOKEN": this.gitlabToken
      },
    })
      .then((response) => response.json())
      .then((data) => {
        this.user = data;
        this.user.website_url_nice = this.user.website_url.substring(8);
      })
      .catch((Error) => {
        console.log(Error);
      });
  }
};
</script>
