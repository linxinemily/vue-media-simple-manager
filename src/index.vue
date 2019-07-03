<template>
  <div>
    <media-uploader @uploadMedia="createMedia" :apiUrl="apiUrl"/>
    <media-gallery
      :medias="medias"
      @deleteMutiMedias="deleteMutiMedias"
      @deleteMedia="deleteMedia"
      @updateMedia="updateMedia"
    />
  </div>
</template>

<script>
import { MediaGallery, MediaUploader } from 'vue-media-simple-manager'

export default {
  components: {
    MediaGallery, MediaUploader
  },
  layout: 'dashboard',
  // mixins: [util],
  data() {
    return {
      medias: [],
      apiUrl: 'http://localhost:8000/api/medias'
    }
  },
  async created() {
    await this.loadMedias()
  },
  methods: {
    async loadMedias() {
      fetch(this.apiUrl, { method: 'GET'})
      .then(res => res.json())
      .then(data => {
        this.medias = data.reverse()
      })
      .catch(e => {
        alert(e)
      })
    },
    async deleteMutiMedias(items) {
      const urls = []
      items.forEach(el => {
        urls.push(`${this.apiUrl}/${el.id}`)
      })
      Promise.all(urls.map(url =>
        fetch(url, {
          method: 'DELETE'
        })
        .then(res => res.json())
        .catch(e => {
          alert(e)
        })
      ))
      .then(data => {
        for (let i = 0; i < data.length; i++) {
          const index = this.medias.findIndex(el => el.id === data[i].id)
          this.medias.splice(index, 1)
        }
      })
      .catch(e => {
        alert(e)
      })
    },
    async updateMedia(item) {
      fetch(`${this.apiUrl}/${item.id}`, {
        method: 'PATCH',
        body: JSON.stringify({
          title: item.title,
          alt: item.alt
        }),
        headers: {
          'Content-Type': 'application/json',
          'Accept': 'application/json'
        }
      })
      .then(res => res.json())
      .then(data => {
        const index = this.medias.findIndex(el => el.id === item.id)
        this.medias[index].title = data.title
        this.medias[index].alt = data.alt
      })
      .catch(e => {
        alert(e)
      })
    },
    async deleteMedia(item) {
      fetch(`${this.apiUrl}/${item.id}`, {
        method: 'DELETE'
      })
      .then(res => res.json())
      .then(data => {
        const index = this.medias.findIndex(el => el.id === item.id)
        this.medias.splice(index, 1)
      })
      .catch(e => {
        alert(e)
      })
    },
    createMedia(data) {
      this.medias.unshift(data)
    }
  }
}
</script>
