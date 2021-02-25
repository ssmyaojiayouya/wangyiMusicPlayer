<template>
  <div class="result-container">
    <div class="title-wrap">
      <!-- 标题 -->
      <h2 class="title">{{$route.query.q}}</h2>
      <span class="sub-title">找到{{total}}个结果</span>
    </div>
    <el-tabs v-model="activeIndex">
      <!-- 歌曲 -->
      <el-tab-pane label="歌曲" name="songs">
        <table class="el-table">
          <thead>
            <th></th>
            <th>音乐标题</th>
            <th>歌手</th>
            <th>专辑</th>
            <th>时长</th>
          </thead>
          <tbody>
            <tr class="el-table__row" v-for="(item,index) in songList" 
                :key="index" @dblclick="playMusic(item.id)">
              <td>{{index+1}}</td>
              <td>
                <div class="song-wrap">
                  <div class="name-wrap">
                    <!-- 歌名 -->
                    <span>{{item.name}}</span>
                    <!-- mv的图标 -->
                    <span class="iconfont icon-mv" v-if="item.mvid != 0" @click="toMvlist(item.mvid)"></span>
                  </div>
                  <!-- 二级描述 -->
                  <span v-if="item.alias.length != 0">{{item.alias[0]}}</span>
                </div>
              </td>
              <!-- 歌手名 -->
              <td>{{item.artists[0].name}}</td>
              <!-- 专辑名 -->
              <td>{{item.album.name}}</td>
              <!-- 歌曲时长 -->
              <td>{{item.duration}}</td>
            </tr>
          </tbody>
        </table>
      </el-tab-pane>
      <!-- 歌单 -->
      <el-tab-pane label="歌单" name="lists" >
        <div class="items">
          <div class="item" v-for="(item,index) in playList" :key="index" @click="toPlaylist(item.id)">
            <div class="img-wrap">
              <div class="num-wrap">
                播放量:
                <span class="num">{{item.playCount}}</span>
              </div>
              <img :src="item.coverImgUrl" alt="" />
              <span class="iconfont icon-play"></span>
            </div>
            <p class="name">{{item.name}}</p>
          </div>
        </div>
      </el-tab-pane>
      <!-- mv -->
      <el-tab-pane label="MV" name="mv">
        <div class="items mv">
          <div class="item" v-for="(item,index) in mvList" :key="index" @click="toMvlist(item.id)">
            <div class="img-wrap">
              <img :src="item.cover" alt="" />
              <span class="iconfont icon-play"></span>
              <div class="num-wrap">
                <div class="iconfont icon-play"></div>
                <div class="num">{{item.playCount}}</div>
              </div>
              <span class="time">{{item.duration}}</span>
            </div>
            <div class="info-wrap">
              <div class="name">{{item.name}}</div>
              <div class="singer">{{item.artistName}}</div>
            </div>
          </div>
        </div>
      </el-tab-pane>
    </el-tabs>

     <!-- 分页器
      total:总条数
      current-page:当前页
      page-size:每页多少数据
     -->
    <el-pagination
      @current-change="handleCurrentChange"
      background
      layout="prev, pager, next"
      :total="total"
      :current-page="page"
      :page-size="10"
    >
    </el-pagination>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'result',
  data() {
    return {
      activeIndex: 'songs',
      // 总条数
      total:0,
      // 页码
      page:1,
      // 一次搜索多少条数据
      limit:10,
      // 搜索类型：歌曲1，歌单1000，MV1004
      type:1,
      
      // 歌曲列表
      songList:[],
      // 歌单列表
      playList:[],
      // mv列表
      mvList:[]
    };
  },
  watch:{
    activeIndex(){
      // songs
      if(this.activeIndex == 'songs') {this.type = 1; this.limit = 30;}
      //lists
      else if(this.activeIndex == 'lists') {this.type = 1000; this.limit = 15;}
      //mv
      else if(this.activeIndex == 'mv') {this.type = 1004; this.limit = 12;}
      this.page = 1
      this.getList()
    }
  },
  created() {
    this.getList()
  },
  methods: {
    // 去歌单详情页
    toPlaylist(id){
      // 跳转并携带数据即可
      this.$router.push('/playlist?q='+id)
    },
    // 去mv详情页
    toMvlist(id){
      // 跳转并携带数据即可
      this.$router.push('/mv?q='+id)
    },
    handleCurrentChange(val) {
      // console.log(`当前页: ${val}`);
      this.page = val
      this.getList()
    },
    playMusic(id){
      axios({
        url:'https://autumnfish.cn/song/url',
        method: 'get',
        params:{
          id
        }
      }).then(res => {
        // console.log(res)
        // console.log(res.data.data[0].url)
        this.musicUrl = res.data.data[0].url
        this.$parent.musicUrl = this.musicUrl  // 更改父组件中的播放链接
      })
    },
    getList(){
      axios({
        url:' https://autumnfish.cn/search',
        params:{
          keywords:this.$route.query.q,
          limit:this.limit,
          offset:(this.page-1)*this.limit,
          type:this.type
        }
      }).then(res => {
        // 获取歌曲
          if(this.type == 1){
            this.songList = res.data.result.songs
            for(let i=0;i < this.songList.length;i++){
              let duration = this.songList[i].duration
              // 把这个毫秒数 转换成分秒
              let min = parseInt(duration/1000/60)
              if(min < 10) {min = '0' + min;}
              let sec = parseInt(duration/1000%60)
              if(sec < 10) {sec = '0' + sec;}
              this.songList[i].duration = `${min}:${sec}`
            }
            // 总数
            this.total = res.data.result.songCount
          }
          // 获取歌单
          else if(this.type == 1000){
            this.playList = res.data.result.playlists
            for(let i=0;i < this.playList.length;i++){
              if(this.playList[i].playCount >= 10000) {
                this.playList[i].playCount = parseInt(this.playList[i].playCount/10000)+'万'
              }
            }
            // 总数
            this.total = res.data.result.playlistCount
          }
          // 获取mv
          else {
            console.log(res)
            this.mvList = res.data.result.mvs
            
            for(let i=0;i < this.mvList.length;i++){
              // 把这个毫秒数 转换成分秒
              let duration = this.mvList[i].duration
              let min = parseInt(duration/1000/60)
              if(min < 10) {min = '0' + min;}
              let sec = parseInt(duration/1000%60)
              if(sec < 10) {sec = '0' + sec;}
              this.mvList[i].duration = `${min}:${sec}`

              // 转换播放量
              if(this.mvList[i].playCount >= 10000) {
                this.mvList[i].playCount = parseInt(this.mvList[i].playCount/10000)+'万'
              }
            }
            // 总数
            this.total = res.data.result.mvCount
          }
          
      })
    },
    
  },
};
</script>

<style >

</style>
