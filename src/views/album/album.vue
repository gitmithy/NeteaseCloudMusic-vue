<template>
  <div>
    <mu-circular-progress :size="90" color="red" v-if="loding" class="loding"/>
    <div v-else>
      <div class="header-wrap">
        <list-title title="专辑"></list-title>
        <div class="album-msg-wrap">
          <div class="album-pic">
            <img v-lazy="album.picUrl">
          </div>
          <div class="album-msg">
            <h3>{{album.name}}</h3>
            <p>歌手：
              <span v-for="item in album.artists" 
                @click="goArtist(item.id)">
                  {{item.name}}
                </span> >
            </p>
            <p>发行时间：{{(new Date(album.publishTime)).getFullYear()}}.{{(new Date(album.publishTime)).getMonth()+1}}.{{(new Date(album.publishTime)).getDate()}}</p>
          </div>
        </div>
        <div class="listData">
          <div>
            <div>
              <span class="icon-collection"></span>
            </div>
            {{info.likedCount}}
          </div>
          <div @click="goComment(data.id)">
            <div>
              <span class="icon-comment"></span>
            </div>
            {{info.commentCount}}
          </div>
          <div>
            <div>
              <span class="icon-share"></span>
            </div>
            {{info.shareCount}}
          </div>
          <div>
            <div>
              <span class="icon-download"></span>
            </div>
            下载
          </div>
        </div>
      </div>
      <music-list :songs="data.songs"></music-list>
    </div>
  </div>
</template>

<script>
  import musicList from '../../components/musicList'
  import listTitle from '../../components/listTitle'
  export default {
    data () {
      return {
        data: '',
        album: '',
        info: '',
        loding: true
      }
    },
    components: {
      listTitle,
      musicList
    },
    mounted () {
      this.$http.get(`http://localhost:3000/album/?id=${this.$route.params.id}`)
        .then((res) => {
          this.data = res.data
          this.album = res.data.album
          this.info = res.data.album.info
          this.loding = false
        })
    },
    methods: {
      goComment () {
        let obj = {
          name: this.album.name,
          artists: this.album.artists,
          musicpic: this.album.picUrl
        }
        this.$store.dispatch('saveCommentHeader', obj)
        this.$store.dispatch('saveCommentType', 2)
        this.$router.push({name: 'comment'})
      },
      goArtist (id) {
        this.$router.push({path: `/artist/${id}`})
      }
    }
  }
</script>

<style lang="scss" scoped>
  .header-wrap {
    background: #df2d2d;
    color: #fff;
  }
  .album-msg-wrap {
    display: flex;
    .album-pic {
      width: 40%;
      padding: 0.2rem;
      img {
        width: 100%;
      }
    }
    .album-msg {
      flex: 1;
      padding: 0.2rem;
    }
  }
  .listData {
    display: flex;
  }
  .listData > div {
    flex: 1;
    text-align: center;
    cursor: pointer;
  }
  .loding {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
  }
</style>
