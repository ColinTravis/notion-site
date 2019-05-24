<template>
  <div class="container">
    <div class="bg-white mx-auto rounded-lg p-6 shadow-md w-2">
      <!-- <img class="h-16 w-16 rounded-full mx-auto" src="avatar.jpg"> -->
      <div class="text-center">
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

    return { ...notionData, etag }
  },

  methods: {
    logVal() {
      console.log('Notion Data', notionData)
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

