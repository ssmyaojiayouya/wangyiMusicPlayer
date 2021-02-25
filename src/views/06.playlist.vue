<template>
  <div class="playlist-container">
    <div class="top-wrap">
      <div class="img-wrap">
        <!-- 图片背景 -->
        <img :src="playList.coverImgUrl" alt="" />
      </div>
      <div class="info-wrap">
        <!-- 歌单名字 -->
        <p class="title">{{playList.name}}</p>
        <div class="author-wrap">
          <img class="avatar" :src="playList.creator.avatarUrl" alt="" />
          <!-- 歌单作者 -->
          <span class="name">{{playList.creator.nickname}}</span>
          <!-- 歌单创建时间 -->
          <span class="time">{{new Date(playList.createTime).toLocaleDateString()}} 创建</span>
        </div>
        <div class="play-wrap">
          <span class="iconfont icon-circle-play"></span>
          <span class="text">播放全部</span>
        </div>
        <div class="tag-wrap">
          <!-- 歌单的标签 -->
          <span class="title">标签:</span>
          <ul v-if="playList.tags.length != 0">
            <li v-for="(item,index) in playList.tags" :key="index">{{item}}</li>
          </ul>
        </div>
        <div class="desc-wrap">
          <span class="title">简介:</span>
          <!-- 歌单的简介 -->
          <span class="desc">{{playList.description}}</span>
        </div>
      </div>
    </div>
    
    <el-tabs v-model="activeIndex">
      <!-- 歌曲列表 -->
      <el-tab-pane label="歌曲列表" name="1">
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
            <!-- 歌曲列表 -->
            <tr class="el-table__row" v-for="(item,index) in playList.tracks" :key="index">
              <td>{{index+1}}</td>
              <td>
                <!--  -->
                <div class="img-wrap" @click="playMusic(item.al.id)">
                  <img :src="item.al.picUrl" alt="" />
                  <span class="iconfont icon-play" ></span>
                </div>
              </td>
              <td>
                <div class="song-wrap">
                  <div class="name-wrap">
                    <!-- 歌曲名 -->
                    <span>{{item.name}}</span>
                    <span class="iconfont icon-mv" v-if="item.mv != 0" @click="toMvlist(item.mv)"></span>
                  </div>
                  
                  <span v-if="item.alia.length != 0">{{item.alia[0]}}</span>
                </div>
              </td>
              <!-- 歌手名 -->
              <td>{{item.ar[0].name}}</td>
              <!-- 专辑名字 -->
              <td>{{item.al.name}}</td>
              <!-- 歌曲时长 -->
              <td>{{item.dt}}</td>
            </tr>
          </tbody>
        </table>
      </el-tab-pane>
      <!-- 评论 -->
      <el-tab-pane :label="showSum()" name="2">
        <!-- 精彩评论 -->
        <div class="comment-wrap">
          <p class="title">精彩评论<span class="number">({{hotCount}})</span></p>
          <div class="comments-wrap">
            <div class="item" v-for="(item,index) in hotComments" :key="index">
              <div class="icon-wrap">
                <!-- 评论人的头像 -->
                <img :src="item.user.avatarUrl" alt="" />
              </div>
              <div class="content-wrap">
                <!-- 评论人的昵称及内容 -->
                <div class="content">
                  <span class="name">{{item.user.nickname}}</span>
                  <span class="comment">{{item.content}}</span>
                </div>
                <!-- 恢复评论的人的昵称和内容 -->
                <div class="re-content" v-if="item.beReplied.length != 0" >
                  <span class="name">{{item.beReplied[0].user.nickname}}</span>
                  <span class="comment">{{item.beReplied[0].content}}</span>
                </div>
                <div class="date">{{(new Date(item.time).toLocaleString())}}</div>
              </div>
            </div>
          </div>
        </div>
        <!-- 最新评论 -->
        <div class="comment-wrap">
          <p class="title">最新评论<span class="number">({{Count}})</span></p>
          <div class="comments-wrap">
            <div class="item" v-for="(item,index) in comments" :key="index">
              <div class="icon-wrap">
                <img :src="item.user.avatarUrl" alt="" />
              </div>
              <div class="content-wrap">
                <div class="content">
                  <span class="name">{{item.user.nickname}}</span>
                  <span class="comment">{{item.content}}</span>
                </div>
                <div class="re-content" v-if="item.beReplied.length != 0">
                  <span class="name">{{item.beReplied[0].user.nickname}}</span>
                  <span class="comment">{{item.beReplied[0].content}}</span>
                </div>
                <div class="date">{{new Date(item.time).toLocaleString()}}</div>
              </div>
            </div>
          </div>
        </div>
        <!-- 分页器 -->
        <el-pagination
          @current-change="handleCurrentChange"
          background
          layout="prev, pager, next"
          :total="total"
          :current-page="page"
          :page-size="5"
        >
        </el-pagination>
      </el-tab-pane>
    </el-tabs>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'playlist',
  data() {
    return {
      activeIndex: '1',
      // 总条数
      total: 0,
      // 页码
      page: 1,
      limit:15,
      // 歌单详情
      playList:{},
      // 热门评论列表
      hotComments:[],
      // 热门评论的个数
      hotCount:0,
      // 最新评论列表
      comments:[],
      Count:0
    };
  },

  created() {
    this.getplayList()
    this.gethotComments()
    this.getComments()
  },

  methods: {
    // 去mv详情页
    toMvlist(id){
      // 跳转并携带数据即可
      this.$router.push('/mv?q='+id)
    },
    showSum(){
      // 评论总数显示：评论(total)
      return '评论('+this.total+')'
    },
    playMusic(id){
      // console.log(id)
      axios({
        url:'https://autumnfish.cn/song/url',
        method: 'get',
        params:{
          id
        }
      }).then(res => {
        // console.log(res)
        console.log(res.data.data[0].url)
        this.musicUrl = res.data.data[0].url
        // 还未实现：抓紧学习vuex和localstorage
        // 这个播放音乐，因为playlist和index组件完全没有关系，所有不能用父传子，子传父的方法
        // 解决办法：1.用 event bus或者本地存储(localstorage)
        //          2.用vuex
        // this.$refs["02.index.vue"].changeMusicUrl(this.musicUrl)
        // this.$parent.musicUrl = this.musicUrl  // 更改父组件中的播放链接
      })
    },
    getplayList(){
      // 获取歌单的详情
      axios({
        url:'https://autumnfish.cn/playlist/detail',
        method: 'get',
        params:{
          id:this.$route.query.q
        }
      }).then(res => {
        console.log(res)
        this.playList = res.data.playlist
        for(let i=0;i < this.playList.tracks.length;i++){
          // 把这个毫秒数 转换成分秒
          let duration = this.playList.tracks[i].dt
          let min = parseInt(duration/1000/60)
          if(min < 10) {min = '0' + min;}
          let sec = parseInt(duration/1000%60)
          if(sec < 10) {sec = '0' + sec;}
          this.playList.tracks[i].dt = `${min}:${sec}`
        }
      })
    },
    gethotComments(){
      // 获取热门评论
      axios({
        url:'https://autumnfish.cn/comment/hot',
        method:'get',
        params:{
          id: this.$route.query.q,
          type:2
        }
      }).then(res => {
        // console.log(res)
        this.hotComments = res.data.hotComments
        this.hotCount = res.data.total
        this.total = this.Count+this.hotCount
      })
    },
    getComments(){
       // 获取最新评论
      axios({
        url:'https://autumnfish.cn/comment/playlist',
        method:'get',
        params:{
          id: this.$route.query.q,
          offset: (this.page-1)*this.limit
        }
      }).then(res => {
        // console.log(res)
        this.comments = res.data.comments
        this.Count = res.data.total
        this.total = this.Count+this.hotCount
      })
    },
    handleCurrentChange(val) {
      // console.log(`当前页: ${val}`);
      this.page = val
      // console.log(this.page)
      this.gethotComments()
      this.getComments()
    }
  }
};
</script>

<style >

</style>
