<template>
  <div>
    <mv-list :allMV="allMV"></mv-list>
  </div>
</template>

<script>
  import mvList from '../../../components/mvList'
  export default {
    data () {
      return {
        allMV: []
      }
    },
    components: {
      mvList
    },
    activated () {
      this.$http.get(`http://localhost:3000/simi/mv?mvid=${this.$route.params.id}`)
        .then((res) => {
          this.allMV = res.data.mvs
        })
    }
  }
</script>

<style lang="scss" scoped>
</style>
