<template>
  <v-container>
    <v-form @submit.prevent="createLink">
      <v-text-field v-model="url" label="Link Url"></v-text-field>
      <v-btn type="submit">Create Link</v-btn>
    </v-form>
    <div v-if="detailLink">
      <v-btn @click="detailLink = null">Back</v-btn>
      <div>Access Links:</div>
      <div>{{JSON.stringify(accessEvents)}}</div>
    </div>
    <div v-else>
      <div>Recent Links:</div>
      <ul>
        <li v-for="shortLink in shortLinks" :key="shortLink.short_url">
          {{`${shortLink.long_url} - `}}
          <a :href="shortLink.short_url" target="_blank" rel="noopener noreferrer">{{shortLink.short_url}}</a>
          {{' - '}}
          <v-btn x-small outlined @click="getAccessEvents(shortLink)">Show Log</v-btn>
        </li>
      </ul>
    </div>

  </v-container>
</template>

<script>
export default {
  data() {
    return {
      shortLinks: [],
      accessEvents: null,
      detailLink: null,
      url: ''
    }
  },
  methods: {
    async createLink() {
      const { data } = await this.$axios.$post('/short_link', { long_url: this.url })
      this.shortLinks = [...this.shortLinks, data]
    },
    async getAccessEvents(shortLink) {
      this.detailLink = shortLink
      const { data } = await this.$axios.$get(`${shortLink.short_url}+`)
      this.accessEvents = data
    }
  }
}
</script>