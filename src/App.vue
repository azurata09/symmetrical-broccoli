<script setup lang="ts">
import { onMounted, ref } from 'vue'
import IdolBox from './components/IdolBox.vue'

const query = `
PREFIX schema: <http://schema.org/>
PREFIX imas: <https://sparql.crssnky.xyz/imasrdf/URIs/imas-schema.ttl#>
select ?利き手 ?名前
where{
 ?s schema:name|schema:alternateName ?name;
    imas:Handedness ?利き手;
 filter(regex(str(?name), "まゆ")).
 ?idol imas:Handedness ?利き手;
       schema:name ?名前.
}
`
const BASE_URL = 'https://sparql.crssnky.xyz/spql/imas/query?output=json&force-accept=text%2Fplain&query='

const ambidextrous_idols = ref<string[]>([])

type Response = {
  results: {
    bindings: {
      名前: {
        value: string
        "xml:lang": string
      }
    }[]
  }
}

onMounted(() => {
  fetch(BASE_URL + encodeURIComponent(query)).then(r => r.json())
  .then((json: Response) => {
    const result = json.results.bindings.filter((i) => i.名前['xml:lang'] === 'ja').map(i => i.名前.value)
    ambidextrous_idols.value = result
  }).catch((err) => {
    console.error(err)
  })
})
</script>

<template>
  <h1>両利きのアイドル一覧</h1>
  <div class="idols" v-if="0 < ambidextrous_idols.length">
    <IdolBox v-for="idol in ambidextrous_idols" :key="idol" :idol="idol" />
  </div>
  <div class="idols" v-else>
    Loading...
  </div>
</template>

<style scoped>
.idols {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
}
</style>
