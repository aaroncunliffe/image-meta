<template>
  <div id="app">
      <b-navbar type="dark" class="justify-content-center">
      <b-navbar variant="faded" type="light">
        <b-navbar-brand tag="h1" class="mb-0">ImageMeta</b-navbar-brand>
      </b-navbar>
    </b-navbar>

    <router-view/>

    <br/>

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

<style lang="scss">
</style>
