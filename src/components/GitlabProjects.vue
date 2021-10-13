<template>
  <div class="box">
    <h3 class="title is-3">Active Projects</h3>
    <article
      v-for="project in projects"
      class="media"
      :href="project.web_url"
      :key="project"
    >
      <div class="media-left" v-if="project.avatar_url">
        <figure class="image is-64x64">
          <img :src="project.avatar_url" />
        </figure>
      </div>
      <div class="media-content">
        <div class="content">
          <h5 class="subtitle is-5 is-spaced">{{ project.name }}</h5>
          <div class="tags" v-if="project.topics">
            <span
              v-for="topic in project.topics"
              :key="topic"
              class="tag is-link"
              >{{ topic }}</span
            >
          </div>
          <small>Last activity: {{ project.last_activity_at }}</small>
          <p v-if="project.description">{{ project.description }}</p>
          <p v-if="project.issues_enabled && project.open_issues_count">
            {{ project.open_issues_count }} open issues
          </p>
          <div class="buttons">
            <a
              target="blank"
              :href="project.web_url"
              class="button is-primary is-rounded"
              >Visit</a
            >
            <a
              target="blank"
              :href="`${project.web_url}/-/issues`"
              class="button is-rounded"
              >Issues</a
            >
            <a
              target="blank"
              :href="`${project.web_url}/-/merge_requests`"
              class="button is-rounded"
              >Merge Requests</a
            >
          </div>
        </div>
      </div>
    </article>
  </div>
</template>

<script>
export default {
  name: "GitlabProjects",
  props: {
    gitlabToken: {
      type: String,
      required: true,
    },
    gitlabId: {
      type: Number,
      required: true,
    },
  },
  data() {
    return {
      projects: {},
    };
  },
  created() {
    fetch(
      `https://gitlab.com/api/v4/users/${this.gitlabId}/projects?archived=false&order_by=last_activity_at`,
      {
        headers: {
          "PRIVATE-TOKEN": this.gitlabToken,
        },
      }
    )
      .then((response) => response.json())
      .then((data) => {
        this.projects = data;
      })
      .catch((Error) => {
        console.log(Error);
      });
  },
};
</script>
