<template>
  <div id="app">
    <b-navbar type="dark" class="justify-content-center">
      <b-navbar variant="faded" type="light">
        <b-navbar-brand tag="h1" class="mb-0">ImageMeta</b-navbar-brand>
      </b-navbar>
    </b-navbar>

    <Meta />
    <Faq />

    <div class="footer">
      <b-container>
        <b-row>
          <b-col>
            <p>Created by <a href="https://aaroncunliffe.dev">Aaron Cunliffe</a></p>
          </b-col>
        </b-row>
      </b-container>
    </div>
  </div>
</template>

<script>
import Meta from "./components/Meta.vue";
import Faq from "./components/Faq.vue";

export default {
  name: "app",
  components: {
    Meta,
    Faq
  }
};
</script>

<style>
.navbar-laravel {
  background-color: #fff;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.04);
}

body {
  background-color: #333;
}

.navbar {
  border: 10px;
  background-color: #444;
}

.navbar-brand {
  color: white !important;
  font-size: 32px;
  font-weight: bold;
}

hr {
  border-color: white;
}

h3,
h4,
h5 {
  margin-bottom: 10px;
  color: white;
}

h2 {
  color: white;
}

li,
p {
  color: white;
}

a {
  color: white;
  text-decoration: underline;
}

.btn {
  text-decoration: none;
}

a:hover {
  color: #444;
}

.btn-default {
  color: white;
  background-color: transparent;
  border: 1px solid white;
  font-size: 20px;
}

.btn-default:focus,
.btn-default:hover {
  color: black;
  border: 1px solid white;
  background-color: white;
}

.footer {
  bottom: 0;
  width: 100%;
  height: 60px;
  line-height: 60px;
  padding-top:30px;
}
</style>
