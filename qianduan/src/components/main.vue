<template>
  <div>
    <el-button @click="drawer = true" type="primary" class="home-button">
      我
    </el-button>
    <el-drawer title="我的主页" :visible.sync="drawer" :with-header="false" :direction="'ltr'">
      <div class="drawer-content">
        <li class="el-icon-s-tools">
          <h1 class="drawer-title">我的主页</h1>
        </li>
        <ul class="drawer-menu">
          <li class="el-icon-caret-right">
            <router-link to="/my">我的文章</router-link>
          </li>
          <br>
          <li class="el-icon-caret-right">
            <router-link to="/addA">创建文章</router-link>
          </li>
          <br>
          <li class="el-icon-caret-right">
            <router-link to="/editprofile">编辑资料</router-link>
          </li>
          <br>
          <li class="el-icon-caret-right">
            <router-link to="/shoucanng">我的收藏</router-link>
          </li>
          <br>
          <li class="el-icon-caret-right">
            <router-link to="/index">切换账号</router-link>
          </li>
          <br>
          <li class="el-icon-caret-right">
            <a @click="Logout" style="cursor: pointer;">退出账号</a>
          </li>
        </ul>
      </div>
    </el-drawer>
    <!-- 查文章 -->
    <form @submit.prevent="submitArticle" class="article-form">
      <div class="form-group">
        <el-button type="primary"   @click="fanhuiArticles">返回</el-button>
        <input type="text" v-model="inputwen.title" required  style="width: 200px; height: 30px;">
        <el-button type="primary" icon="el-icon-search"  @click="searchArticles">搜索</el-button>
        
      </div>
    </form>
    <div class="hello">
      <h1 class="title">{{ msg }}</h1>
      <!-- 文章列表 -->
      <ul class="article-list">
        <li v-for="(wen, index) in filteredArticles" :key="wen.id" class="article-item">
          <router-link :to="'/showdetails/' + wen.xu" class="article-link">
            <span class="article-index">{{ wen.xu }}</span>
            <span class="article-title">{{ wen.title }}</span>
            <span class="article-author">{{ wen.author }}</span>
          </router-link>
        </li>
      </ul>


    </div>
  </div>
</template>

<script>
import { getArticles, logout, postwen ,paixu} from '../api/api.js'

export default {
  name: 'HelloWorld',
  data() {
    return {
      msg: '请欣赏我们的文章',
      wens: [],
      drawer: false,
      inputwen: {
        author: '',
        title: '',
        content: '',
        xu:''

      },
      filteredArticles: [], // 用于存储筛选后的文章
    }
  },
  methods: {
    loadArticles() {
      getArticles().then(response => {
        this.wens = response.data;
        this.filteredArticles = response.data; // 初始化 filteredArticles
      });
    },
    submitArticle() {
      // 提交文章表单
      postwen(this.inputwen.author, this.inputwen.title, this.inputwen.content).then(response => {
        console.log(response);
        this.loadArticles();
        this.inputwen = { author: '', title: '', content: '' }; // 清空表单
      });
    },
    searchArticles() {
      // 在 wens 中筛选包含搜索关键字的文章
      this.filteredArticles = this.wens.filter(wen => wen.title.includes(this.inputwen.title));
    },
    fanhuiArticles() {
      // 在 wens 中筛选包含搜索关键字的文章
      this.filteredArticles = this.wens;
      this.inputwen.title=""
    },
    Logout() {
      logout().then(response => {
        this.$message({
          message: response.data.msg,
          type: response.data.msg === '退出成功' ? 'success' : 'error',
        });
        if (response.data.msg === '退出成功') {
          this.$router.push({ name: 'index' });
        }
      });
    },
      pai() {
      paixu().then(() => {
       alert("paixuwancheng");
       this.loadArticles();
      });
      }
  },
  created() {
    this.loadArticles();
  }
}
</script>




<style>



.title {
  font-size: 24px;
  font-weight: bold;
  color: #333;
  margin-bottom: 20px;
}



.form-group {
  margin-bottom: 10px;
}



.article-list {
  list-style: none;
  padding: 0;
}

.article-item {
  margin-bottom: 10px;
  padding: 10px;
  background-color: #f9f9f9;
  border: 1px solid #ccc;
  border-radius: 5px;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.article-link {
  text-decoration: none;
  color: #333;
  display: flex;
  align-items: center;
}

.article-index {
  flex: 0 0 10%;
  font-weight: bold;
}
.article-title {
  width: 100%;
  text-align: center;
  font-size: 18px;
}


.article-author {
  flex: 0 0 30%;
  font-size: 16px;
  color: #777;
  align-items: center;
}

.home-button {
  position: absolute;
  top: 10px;
  left: 10px;
  background-color: #007BFF;
  color: #fff;
  padding: 10px 20px;
  border: none;
  border-radius: 5px;
  text-decoration: none;
}



.drawer-content {
  padding: 20px;
}

.drawer-title {
  font-size: 24px;
  margin-bottom: 20px;
}

.drawer-menu {
  list-style: none;
  padding: 0;
}

.drawer-menu li {
  margin-bottom: 10px;
}

.drawer-menu a {
  text-decoration: none;
  color: #333;
}
</style>
