<template>
  <div class="article-detail">
    <el-breadcrumb separator-class="el-icon-arrow-right">
      <el-breadcrumb-item :to="{ path: '/main' }">首页</el-breadcrumb-item>
      <el-breadcrumb-item>文章欣赏</el-breadcrumb-item>
      
    </el-breadcrumb>
    <br>
    <router-link to="/main" class="back-button">
      <el-page-header @back="goBack" content="文章详情">
</el-page-header>
    </router-link>

    <el-alert v-if="d2 === 1" title="已经是最后一篇了" type="warning" @close="hello"></el-alert>
    <el-alert v-if="d1 === 1" title="已经是第一篇了" type="warning" @close="hello"></el-alert>
    <h1 class="article-title">{{ article.title }}</h1>
    <p class="article-info">作者：{{ article.author }}</p>
    <p class="article-content">{{ article.content }}</p>
    <el-button-group>
      <el-button type="primary" icon="el-icon-arrow-left" @click="showPreviousArticle">上一篇</el-button>
      <el-button type="primary" @click="showNextArticle">下一篇<i class="el-icon-arrow-right el-icon--right"></i></el-button>
    </el-button-group>
  </div>
</template>

<script>
import { detailed } from '../api/api.js';

export default {
  data() {
    return {
      d1: 0,
      d2: 0,
      page: this.$route.params.id,
      article: {
        title: '',
        author: '',
        content: '',
      },
    };
  },
  created() {
    this.getDetail();
  },
  methods: {
    hello() {
    },
    showPreviousArticle() {
      if (this.page > 1) {
        this.d1 = 0;
        this.d2 = 0;
        this.page--;
        this.getDetail();

      }
      else {
        this.d1 = 1;
      }
    },
    showNextArticle() {
      this.d1 = 0;
      this.d2 = 0;
      this.page++;

      this.getDetail();
    },
    getDetail() {
      detailed(this.page)
        .then((response) => {
          this.article = response.data;
        })
        .catch((error) => {
          console.error(error);
          this.d2 = 1;
          this.page--;

        });
    },
  },
};
</script>


<style>
.article-detail {
  margin: 20px;
  padding: 20px;
  background-color: #f5f5f5;
  /* 浅灰色背景 */
  border: 1px solid #ccc;
  border-radius: 8px;
  /* 更圆润的边框 */
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  /* 更明显的阴影 */
}

.article-title {
  font-size: 28px;
  /* 增大标题字体 */
  font-weight: bold;
  color: #333;
  margin-bottom: 20px;
  /* 增加标题下方间距 */
}

.article-info {
  font-size: 18px;
  /* 增大作者信息字体 */
  color: #555;
  /* 更柔和的文本颜色 */
  margin-bottom: 10px;
}

.article-content {
  font-size: 20px;
  /* 增大内容字体 */
  line-height: 1.6;
  color: #333;
  /* 更醒目的文本颜色 */
  margin-top: 20px;
}

.back-button {
  color: #007BFF;
  text-decoration: none;
  display: flex;
  align-items: center;
  font-size: 18px;
  /* 增大返回按钮字体 */
  margin-bottom: 20px;
  /* 增加按钮下方间距 */
}

.back-button i {
  margin-right: 8px;
  /* 缩小图标与文本间距 */
  font-size: 24px;
  /* 增大图标大小 */
}
</style>
