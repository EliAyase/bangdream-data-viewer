<template>
  <music :data="currMusic" :diffi="currDiffi"></music>
</template>

<script>
import { mapGetters } from 'vuex'
import Music from './common/Music'

export default {
  name: 'musicDetailComponent',
  components: {
    Music
  },
  computed: {
    ...mapGetters('DB', [
      'musicList',
      'musicDifficultyList'
    ]),
    currMusic () {
      if (!this.musicList) return {}
      return this.musicList.find(music => music.id === this.$route.params.musicID)
    },
    currDiffi () {
      if (!this.musicDifficultyList) return {}
      return this.musicDifficultyList.find(music => music.musicID === this.$route.params.musicID)
    }
  }
}
</script>

<style lang="stylus">

</style>
