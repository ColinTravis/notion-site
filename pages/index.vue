<template>
  <div class="flex-column content-center justify-center">
    <div class="bg-white rounded-lg w-50 p-2 shadow-md" :style="{ background: meta.color[0][0]}" >
      <!-- <img class="h-16 w-16 rounded-full mx-auto" src="avatar.jpg"> -->
      <div class="text-center">
        <!-- <h1>{{notionData}}</h1> -->
        <h1>{{sections[0].title[0][0]}}</h1>
        <h3>{{meta.title[0][0]}}</h3>
        <!-- <p>{{sections}}</p> -->
        <!-- <h1>Title: {{notionData.meta.title[0][0]}}</h1> -->
        <!-- <h1>Description: {{notionData.meta.description[0][0]}}</h1> -->
        <!-- <h1>Color: {{notionData.meta.color[0][0]}}</h1> -->
        <h2 class="text-lg"></h2>
        <div class="text-purple-500"></div>
        <div class="text-gray-600"></div> 
        <div class="text-gray-600"></div>
      </div>
    </div>

  <ul v-for="(section, index) in sections" :key="index">
    <li class="mt-5">{{section}}</li>
  </ul>

  </div>
</template>


<script>
import getNotionData from '~/plugins/notion'

export default {
  data() {},

  async asyncData({ res }) {
    const notionData = await getNotionData()
    // const etag = await generateETag(JSON.stringify(notionData))
    const etag = require('crypto')
      .createHash('md5')
      .update(JSON.stringify(notionData))
      .digest('hex')
    if (res) {
      res.setHeader('Cache-Control', 's-maxage=1, stale-while-revalidate')
      res.setHeader("X-version", etag)
    }
    // console.log('Notion Data', notionData)
    // console.log('Etag', etag)
    return {
      notionData,
      sections: notionData.sections,
      meta: notionData.meta,
      etag: etag
    }
  },

  methods: {},
    mounted () {
    fetch(window.location, {
          headers: {
            pragma: "no-cache"
          }
        }).then(res => {
          if (res.ok && res.headers.get("x-version") !== this.etag) {
            window.location.reload()
          }
    })
  },

  // async asyncData() {
  //   const { data } = await api.post(
  //     'https://www.notion.so/api/v3/loadPageChunk',
  //     JSON.stringify({
  //       pageId: 'd1c555e0-3886-4c56-b09a-4a4bb879b841',
  //       limit: 50,
  //       cursor: {
  //         stack: []
  //       },
  //       chunkNumber: 0,
  //       verticalColumns: false
  //     }),
  //     {
  //       headers: {
  //         'Content-Type': 'application/json',
  //         Accept: 'application/json'
  //       }
  //     }
  //   )
  //   return { about }
  // },

  // async asyncData() {
  //     if (process.browser && process.static) {
  //         const { data: about } = await api.get('/api/singletons/get/About')
  //         return { about }
  //     } else {
  //         const {data: about} = await api.post('/api/singletons/get/About',
  //             JSON.stringify({
  //                 token: process.env.collectionsAPI
  //             }),
  //             {
  //                 headers: {
  //                     'Content-Type': 'application/json',
  //                     'Accept': 'application/json',
  //                 }
  //             });
  //         return {about}
  //     }
  // },

  head() {
    return {
      title: this.meta.title[0][0],
    meta: [
      { charset: 'utf-8' },
      { name: 'viewport', content: 'width=device-width, initial-scale=1' },
      { hid: 'description', name: 'description', content: this.meta.description[0][0] }
    ]
    }
  }
}
</script>