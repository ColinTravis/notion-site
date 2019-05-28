<template>
  <div class="flex content-center justify-center">
    <div class="bg-white rounded-lg w-50 p-2 shadow-md">
      <!-- <img class="h-16 w-16 rounded-full mx-auto" src="avatar.jpg"> -->
      <div class="text-center">
        <h1>Title: {{notionData.meta.title[0][0]}}</h1>
        <h1>Description: {{notionData.meta.description[0][0]}}</h1>
        <h1>Color: {{notionData.meta.color[0][0]}}</h1>
        <h2 class="text-lg"></h2>
        <div class="text-purple-500"></div>
        <div class="text-gray-600"></div>
        <div class="text-gray-600"></div>
      </div>
    </div>
  </div>
</template>

<script>
import getNotionData from '~/plugins/notion'

export default {
  data() {},
  async asyncData({ res }) {
    const notionData = await getNotionData()
    const etag = require('crypto')
      .createHash('md5')
      .update(JSON.stringify(notionData))
      .digest('hex')

    if (res) {
      res.setHeader('Cache-Control', 's-maxage=1, stale-while-revalidate')
      res.setHeader('X-version', etag)
    }
      console.log('Notion Data', notionData)
    return { notionData, etag }
  },

  methods: {
    logVal() {
    }
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

  head: {
    title: 'about'
  }
}
</script>

