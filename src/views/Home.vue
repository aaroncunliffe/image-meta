<template>
  <div class="home">
    <Meta />
    <Info />
  </div>
</template>

<script lang="ts">
import { Component, Vue } from 'vue-property-decorator'

import Meta from '@/components/Meta.vue'
import Info from '@/components/Info.vue'

@Component({
  components: {
    Meta,
    Info
  }
})
export default class Home extends Vue {}
</script>
