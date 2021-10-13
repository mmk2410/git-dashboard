<template>
  <div class="card">
    <div class="card-content">
      <p class="title is-4">GitLab Authentication required</p>
      <p class="mb-5">
        Please create a GitLab authentication token over at
        <a
          href="https://gitlab.com/-/profile/personal_access_tokens"
          taget="blank"
          >GitLab.com</a
        >
        (API scope is enought) and enter it here. Attention: Currently the token
        is not stored in any way by this app so it is recommended to save in a
        safe place e.g. a password store for the next use.
      </p>
      <div class="field has-addons has-addons-centered">
        <p class="control">
          <input
            @keyup.enter="$emit('authTokenSubmit', authToken)"
            v-model="authToken"
            class="input"
            type="text"
            placeholder="GitLab Access Token"
          />
        </p>
        <p class="control">
          <a @click="$emit('authTokenSubmit', authToken)" class="button is-primary">Submit</a>
        </p>
      </div>
      <div v-if="authError" class="notification is-danger">
          <strong>Authentication failed!</strong> Please double check your authentication token.
      </div>
    </div>
  </div>
</template>

<script>
export default {
    name: "GitlabAuth",
    props: ['authError'],
    emits: ['authTokenSubmit'],
    data() {
        return {
            authToken: ""
        }
    }
};
</script>
