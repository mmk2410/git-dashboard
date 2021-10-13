<template>
  <h1 class="title">My GitLab Dashboard</h1>
  <gitlab-auth
    v-if="!authorized"
    @authTokenSubmit="authTokenSubmit"
    :auth-error="authError"
  ></gitlab-auth>
  <div v-if="authorized" class="columns">
    <div class="column is-one-quarter">
      <gitlab-user :gitlabToken="gitlabToken"></gitlab-user>
    </div>
    <div class="column">
      <gitlab-projects
        :gitlabToken="gitlabToken"
        :gitlabId="gitlabId"
      ></gitlab-projects>
    </div>
  </div>
</template>

<script lang="ts">
import { defineComponent } from "vue";
import GitlabAuth from "./components/GitlabAuth.vue";
import GitlabUser from "./components/GitlabUser.vue";
import GitlabProjects from "./components/GitlabProjects.vue";

export default defineComponent({
  name: "App",
  components: {
    GitlabAuth,
    GitlabUser,
    GitlabProjects,
  },
  data() {
    return {
      gitlabToken: "",
      gitlabId: 0,
      authorized: false, // temp property, should be replaced by an empty gitlabToken
      authError: false,
    };
  },
  methods: {
    authTokenSubmit(authToken: string) {
      if (!authToken) return;

      fetch("https://gitlab.com/api/v4/user", {
        headers: {
          "PRIVATE-TOKEN": authToken,
        },
      })
        .then((response) => response.json())
        .then((data) => {
          if (data.id) {
            this.gitlabId = data.id;
          } else {
            throw new Error("Auth token not valid");
          }
          this.gitlabToken = authToken;
          this.authorized = true;
        })
        .catch((Error) => {
          console.log(Error);
          this.authError = true;
        });
    },
  },
});
</script>
