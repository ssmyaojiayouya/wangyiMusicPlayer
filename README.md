## wangyiMusicPlayer——这是一个仿网易云PC端的的项目

## 项目简介：
本项目使用的**后端接口**{接口文档已放在项目中，自行下载使用}

**前端采用技术**：
1.vue-cli,vue-router,element-ui,axios请求,父子组件传值

2.路由跳转（携带参数）：具体体现在各个页面的跳转

3.组件从本身跳转到本身（携带参数）：具体体现在mv详情页中(mv.vue) 点击 推荐mv

### 项目效果图：

- **发现音乐**

![image-20210225141513450](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210225141513450.png)

- **推荐歌单**

![image-20210225140644867](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210225140644867.png)

- **最新音乐**

### ![image-20210225140711219](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210225140711219.png)

- **最新mv**

![image-20210225140742272](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210225140742272.png)

- **歌单详情页**

![image-20210225140829213](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210225140829213.png)

![image-20210225141405387](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210225141405387.png)



- **mv详情页**

![image-20210225141150452](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210225141150452.png)

![image-20210225141224136](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210225141224136.png)

- **搜索关键字后的结果页**

![image-20210225141112599](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210225141112599.png)

![image-20210225141024956](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210225141024956.png)

![image-20210225141046547](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210225141046547.png)

### 项目的启动：

- 首先创建一个vue-cli项目：vue create hmplayer
- 进入到项目目录下，执行npm run serve命令

![image-20210225141842914](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210225141842914.png)



### 项目目录介绍

<img src="C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20210225143113395.png" alt="image-20210225143113395" style="zoom: 67%;" />

### 项目的不足

- 歌单详情里面的  播放全部按钮还未实现
- 在歌单详情页面里不能播放音乐，只能播放mv，是因为播放音乐的标签写在了index.vue中，而歌单详情页面playlist.vue与index.vue是平级，没有联系。要实现无关系的平级组件之间修改属性，可能要用vuex实现 或者 用 event bus或者本地存储(localstorage) 
- 抓紧学习，争取完善更新整个项目，做出更复杂的功能需求，加油加油加油！！！！

