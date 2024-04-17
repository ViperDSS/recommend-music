<template>
  <div class="home">
    <!--轮播图-->
    <swiper :swiperList="swiperList"/>
    <!--热门歌单-->
    <div class="section">
      <div class="section-title">歌单</div>
      <content-list :contentList="songList" path="song-list-album"></content-list>
    </div>
    <!--热门歌手-->
    <div class="section">
      <div class="section-title">歌手</div>
      <content-list :contentList="singerList" path="singer-album"></content-list>
    </div>
    <div class="section">
        <album-content :songList="song">
          <template slot="title">热门单曲</template>
        </album-content>
      </div>

  </div>
</template>

<script>
import { swiperList } from '../assets/data/swiper'
import Swiper from '../components/Swiper'
import ContentList from '../components/ContentList'
import AlbumContent from '../components/AlbumContent.vue'
import { getSongList, getAllSinger ,getAllSong} from '../api/index'
import {mapGetters} from 'vuex'
import { get } from '../api/http'

export default {
  name: 'home',
  components: {
    Swiper,
    ContentList,
    AlbumContent
  },
  data () {
    return {
      swiperList: [], // 轮播图列表
      songList: [], // 歌单列表
      singerList: [], // 歌手列表
      song: [] // 歌曲列表
    }
  },
  computed: {
    ...mapGetters([
      'userId'
    ])
  },
  created () {
    this.swiperList = swiperList
    // 获取歌单列表
    // eslint-disable-next-line no-undef
    this.getSongList(this.userId)
    // 获取歌手列表
    this.getSingerList()
    //获取歌曲
    this.getAllSong1()
  },
  methods: {
    getSongList (userId) {
      getSongList(userId)
        .then(res => {
          this.songList = res.sort().slice(0, 10)
        })
        .catch(err => {
          console.log(err)
        })
    },
    getSingerList () {
      getAllSinger()
        .then(res => {
          this.singerList = res.sort().slice(0, 10)
        })
        .catch(err => {
          console.log(err)
        })
    },
    getAllSong1 () {
      getAllSong()
        .then(res => {
          this.song = res.sort().slice(0, 10)
        })
        .catch(err => {
          console.log(err)
        })
    }
  }
}
</script>

<style lang="scss" scoped>
@import '../assets/css/home.scss';
</style>
