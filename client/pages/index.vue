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
        <ShortLink v-for="shortLink in shortLinks" :key="shortLink.short_url" :shortLink="shortLink" @showLog="getAccessEvents(shortLink)"></ShortLink>
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
      const data = await this.$axios.$post('/short_link', { long_url: this.url })
      this.shortLinks = [...this.shortLinks, data]
    },
    async getAccessEvents(shortLink) {
      this.detailLink = shortLink
      const data = await this.$axios.$get(`${shortLink.stub}+`)
      this.accessEvents = data
    }
  }
}
</script>