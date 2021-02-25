## wangyiMusicPlayer——这是一个仿网易云PC端的的项目

## 项目简介：
本项目使用的**后端接口**{接口文档已放在项目中，自行下载使用}

**前端采用技术**：
1.vue-cli,vue-router,element-ui,axios请求,父子组件传值

2.路由跳转（携带参数）：具体体现在各个页面的跳转

3.组件从本身跳转到本身（携带参数）：具体体现在mv详情页中(mv.vue) 点击 推荐mv

### 项目效果图：

- **发现音乐**

![](https://img-blog.csdnimg.cn/20210225145527786.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1bWlzdTY2Ng==,size_16,color_FFFFFF,t_70)

- **推荐歌单**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210225145612672.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1bWlzdTY2Ng==,size_16,color_FFFFFF,t_70)

- **最新音乐**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210225145642617.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1bWlzdTY2Ng==,size_16,color_FFFFFF,t_70)

- **最新mv**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210225145806837.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1bWlzdTY2Ng==,size_16,color_FFFFFF,t_70)

- **歌单详情页**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210225145819598.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1bWlzdTY2Ng==,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210225145833843.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1bWlzdTY2Ng==,size_16,color_FFFFFF,t_70)



- **mv详情页**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210225145850598.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1bWlzdTY2Ng==,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210225145916946.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1bWlzdTY2Ng==,size_16,color_FFFFFF,t_70)

- **搜索关键字后的结果页**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210225145948605.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1bWlzdTY2Ng==,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210225150017176.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1bWlzdTY2Ng==,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210225150030602.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1bWlzdTY2Ng==,size_16,color_FFFFFF,t_70)

### 项目的启动：

- 首先创建一个vue-cli项目：vue create hmplayer
- 进入到项目目录下，执行npm run serve命令

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210225150044289.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1bWlzdTY2Ng==,size_16,color_FFFFFF,t_70)



### 项目目录介绍

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210225150105585.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3N1bWlzdTY2Ng==,size_16,color_FFFFFF,t_70)

### 项目的不足

- 歌单详情里面的  播放全部按钮还未实现
- 在歌单详情页面里不能播放音乐，只能播放mv，是因为播放音乐的标签写在了index.vue中，而歌单详情页面playlist.vue与index.vue是平级，没有联系。要实现无关系的平级组件之间修改属性，可能要用vuex实现 或者 用 event bus或者本地存储(localstorage) 
- 抓紧学习，争取完善更新整个项目，做出更复杂的功能需求，加油加油加油！！！！

