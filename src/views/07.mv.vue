<template>
  <div class="mv-container">
    <div class="mv-wrap">
      <h3 class="title">mv详情</h3>
      <!-- mv -->
      <div class="video-wrap">
        <video
          controls
          autoplay
          :src="mvUrl"
        ></video>
      </div>
      <!-- mv信息 -->
      <div class="info-wrap">
        <div class="singer-info">
          <div class="avatar-wrap">
            <img :src="artist.picUrl" alt="" />
          </div>
          <span class="name">{{artist.name}}</span>
        </div>
        <div class="mv-info">
          <h2 class="title">{{mvDetail.name}}</h2>
          <span class="date">发布：{{mvDetail.publishTime}}</span>
          <span class="number">播放：{{mvDetail.playCount}}次</span>
          <p class="desc">
            {{mvDetail.desc}}
          </p>
        </div>
      </div>
      <!-- 精彩评论 -->
      <div class="comment-wrap" v-if="hotComments.length != 0">
        <p class="title">精彩评论<span class="number">({{hotComments.length}})</span></p>
        <div class="comments-wrap" >
          <div class="item" v-for="(item,index) in hotComments" :key="index">
            <div class="icon-wrap">
              <img :src="item.user.avatarUrl" alt="" />
            </div>
            <div class="content-wrap">
              <div class="content">
                <span class="name">{{item.user.nickname}}：</span>
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
      <!-- 最新评论 -->
      <div class="comment-wrap">
        <p class="title">最新评论<span class="number">({{total}})</span></p>
        <div class="comments-wrap">
          <div class="item" v-for="(item,index) in comments" :key="index">
            <div class="icon-wrap">
              <img :src="item.user.avatarUrl" alt="" />
            </div>
            <div class="content-wrap">
              <div class="content">
                <span class="name">{{item.user.nickname}}：</span>
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
        :limit="limit"
      >
      </el-pagination>
    </div>
    <!-- 相关推荐 -->
    <div class="mv-recommend">
      <h3 class="title">相关推荐</h3>
      <div class="mvs">
        <div class="items">
          <div class="item" v-for="(item,index) in mvs" :key="index" @click="toMvlist(item.id)">
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
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  name: 'mv',
  data() {
    return {
      // 总条数
      total: 20,
      // 页码
      page: 1,
      // 页容量
      limit: 10,
      // mv的id
      id: this.$route.query.q,
      // 歌手的信息
      artist:{},
      // mv的地址
      mvUrl:"",
      // mv的信息
      mvDetail:{},
      // 相关mv
      mvs:[],
      // 热门评论
      hotComments:[],
      // 最新评论
      comments:[],
      total:0
    };
  },
  watch:{
    id(){
      this.loadPage()
      this.page = 1
    }
  },
  created() {
    this.loadPage()
  },
  methods: {
    handleCurrentChange(val) {
      console.log(`当前页: ${val}`);
      this.page = val
      // 请求offset最新评论
      axios({
        url:'https://autumnfish.cn/comment/mv',
        method:'get',
        params:{
          id: this.id,
          limit:this.limit,
          offset:(this.page-1)*this.limit
        }
      }).then(res => {
        console.log(res)
        this.comments = res.data.comments
      })
    },
    // 去mv详情页
    toMvlist(id){
      // 跳转并携带数据即可
      this.id = id // 更改这个页面的全局mv的id，然后用watch监听id的变化，有变化则重新加载一次页面
      this.$router.push('/mv?q='+id)
    },
    loadPage(){
      // 获取mv的地址
      axios({
        url:'https://autumnfish.cn/mv/url',
        method:'get',
        params:{
          id: this.id
        }
      }).then( res=> {
        console.log(res)
        this.mvUrl = res.data.data.url
      })
      // 获取mv信息
      axios({
        url:'https://autumnfish.cn/mv/detail',
        method:'get',
        params:{
          mvid:this.id
        }
      }).then( res=> {
        // console.log(res)
        // 歌曲信息
        this.mvDetail = res.data.data
        if(this.mvDetail.playCount >= 10000) {
          this.mvDetail.playCount = parseInt(this.mvDetail.playCount/10000)+'万'
        }
        // 歌手id
        let singleId = res.data.data.artists[0].id
        // 获取歌手信息
        axios({
          url:'https://autumnfish.cn/artists',
          method:'get',
          params:{
            id:singleId
          }
        }).then( res=> {
          console.log(res)
          this.artist = res.data.artist
        })
      })
      // 获取相关推荐mv
      axios({
        url:'https://autumnfish.cn/simi/mv',
        method:'get',
        params:{
          mvid:this.id
        }
      }).then( res=> {
        console.log(res)
        this.mvs = res.data.mvs
        for(let i=0;i < this.mvs.length;i++){
          if(this.mvs[i].playCount >= 10000) {
            // 转换播放量
            this.mvs[i].playCount = parseInt(this.mvs[i].playCount/10000)+'万'
            // 把这个毫秒数 转换成分秒
            let duration = this.mvs[i].duration
            let min = parseInt(duration/1000/60)
            if(min < 10) {min = '0' + min;}
            let sec = parseInt(duration/1000%60)
            if(sec < 10) {sec = '0' + sec;}
            this.mvs[i].duration = `${min}:${sec}`
          }
        }
      })
      // 获取歌曲评论部分
      axios({
        url:'https://autumnfish.cn/comment/mv',
        method:'get',
        params:{
          id: this.id,
          limit:this.limit,
          offset:(this.page-1)*this.limit
        }
      }).then(res => {
        console.log(res)
        this.hotComments = res.data.hotComments
        this.comments = res.data.comments
        this.total = res.data.total
      })
    }
  }
};
</script>

<style></style>
