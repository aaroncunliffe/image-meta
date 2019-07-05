<template>
  <b-container>
    <b-row>
      <b-col class="pt-4">
        <h5>Information:</h5>
        <p>
          As someone that is into photography it is interesting to review images taken and be able to see the the
          hidden information to see what lens and settings were used to take the image and it is also a great opportunity for a project.
          this makes use of an npm package that I created:
        </p>
      </b-col>
    </b-row>

    <b-row>
      <b-col class="pt-2">
        <h5 class="pb-2">NPM package links:</h5>
        <a
          href="https://www.npmjs.com/package/image-meta-js"
          onmousedown="event.preventDefault()"
          target="_blank"
          class="btn btn-default mr-2"
        >
          <i class="fab fa-npm fa-fw"></i>
          <span class="button-text">npm</span>
        </a>

        <a
          href="https://www.github.com/aaroncunliffe/image-meta-js"
          onmousedown="event.preventDefault()"
          target="_blank"
          class="btn btn-default ml-2"
        >
          <i class="fab fa-github-square fa-fw"></i>
          <span class="button-text">Github</span>
        </a>
        
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
export default {
  name: "Faq",
  data: function() {
    return {};
  },
  methods: {},
  updated() {}
};
</script>

<style scoped>
img {
  max-width: 60%;
}

.button-text {
  padding-left: 0.2rem;
}
</style>
