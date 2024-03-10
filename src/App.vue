<template>
  <div
    class="flex min-h-screen gap-5 w-max"
  >
    <ComicList
      :comicbooks="comicbooks"
      @refresh="refreshComics" 
    />
    <CreateComic @refresh="refreshComics" />
  </div>
</template>

<script>
import ComicList from './components/ComicList.vue'
import CreateComic from './components/CreateComic.vue'
import './assets/styles.css'

export default {
  name: 'App',
  components: {
    ComicList,
    CreateComic
  },
  data(){
    return {
      comicbooks: []
    }
  },
  async created () {
    const response = await fetch('http://localhost:3000/comicbooks')
    const data = await response.json()
    console.log('ruby api data =>', data)
    this.comicbooks = data
  },
  methods: {
    async refreshComics(){
      const response = await fetch('http://localhost:3000/comicbooks')
      const data = await response.json()
      console.log('ruby api data =>', data)
      this.comicbooks = data 
    }
  }
}
</script>
