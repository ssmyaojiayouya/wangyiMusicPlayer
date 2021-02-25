<template>
  <div class="songs-container">
    <div class="tab-bar">
      <span class="item" :class="{active: tag==0}" @click="tag=0" >全部</span>
      <span class="item" :class="{active: tag==7}" @click="tag=7" >华语</span>
      <span class="item" :class="{active: tag==96}" @click="tag=96" >欧美</span>
      <span class="item" :class="{active: tag==8}" @click="tag=8" >日本</span>
      <span class="item" :class="{active: tag==16}" @click="tag=16" >韩国</span>
    </div>
    <!-- 底部的table -->
    <table class="el-table playlit-table">
      <thead>
        <th></th>
        <th></th>
        <th>音乐标题</th>
        <th>歌手</th>
        <th>专辑</th>
        <th>时长</th>
      </thead>
      <tbody>
        <tr class="el-table__row" v-for="(item,index) in songList" :key="index">
          <td>{{index+1}}</td>
          <td>
            <div class="img-wrap">
              <img :src="item.album.picUrl" alt="" />
              <span class="iconfont icon-play" @click="playMusic(item.id)"></span>
            </div>
          </td>
          <td>
            <div class="song-wrap">
              <div class="name-wrap">
                <span>{{item.name}}</span>
                <span class="iconfont icon-mv" @click="toMvlist(item.mvid)" v-if="item.mvid != 0"></span>
              </div>
              <span></span>
            </div>
          </td>
          <td>{{item.album.artists[0].name}}</td>
          <td>{{item.album.name}}</td>
          <td>{{item.duration}}</td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'songs',
  data() {
    return {
      songList:[],
      tag:0,
      musicUrl:""
    };
  },
  // 侦听器
  watch:{
    tag(){
      console.log(this.tag)
      this.getsongList()
    }
  },
  created() {
    this.getsongList()
  },
  methods: {
    // 去mv详情页
    toMvlist(id){
      // 跳转并携带数据即可
      this.$router.push('/mv?q='+id)
    },
    getsongList(){
      // 获取 最新音乐数据
      axios({
        url:'https://autumnfish.cn/top/song',
        method: 'get',
        params: {
          type: this.tag
        }
      }).then(res => {
        console.log(res.data.data)
        this.songList = res.data.data
        for(let i=0;i < this.songList.length;i++){
          let duration = this.songList[i].duration
          // 把这个毫秒数 转换成分秒
          let min = parseInt(duration/1000/60)
          if(min < 10) {min = '0' + min;}
          let sec = parseInt(duration/1000%60)
          if(sec < 10) {sec = '0' + sec;}
          this.songList[i].duration = `${min}:${sec}`
        }
      })
    },
    playMusic(id){
      axios({
        url:'https://autumnfish.cn/song/url',
        params:{
          id
        }
      }).then(res => {
        // console.log(res.data.data[0].url)
        this.musicUrl = res.data.data[0].url
        this.$parent.musicUrl = this.musicUrl
      })
    }
  },
};
</script>

<style >

</style>
