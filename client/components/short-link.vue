<template>
  <li>
    {{`${shortLink.long_url} - `}}
    <a :href="shortLink.short_url" target="_blank" rel="noopener noreferrer">{{shortLink.short_url}}</a>
    {{' - '}}
    <v-btn x-small outlined @click="$emit('showLog')">Show Log</v-btn>
  </li>
</template>

<script>
  export default {
    props: ['shortLink']
  }
</script>
