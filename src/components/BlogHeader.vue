/* 页眉组件 */
<template>
  <div id="header">
    <div class="grid">
      <div class="title">
        <h1>
          <router-link to="/">
            <button>第一组博客</button>
          </router-link>
        </h1>
      </div>

      <!--引入搜索框组件-->
      <SearchButton />
      <div class="login">
        <div v-if="hasLogin">
          <div class="dropdown">
            <button class="dropbtn">欢迎, {{ username }}!</button>
            <div class="dropdown-content">
              <router-link
                :to="{ name: 'UserCenter', params: { username: username } }"
                >用户中心</router-link
              >
              <router-link :to="{ name: 'ArticleCreate' }" v-if="isSuperuser"
                >发表文章</router-link
              >
              <router-link to="/" v-on:click.prevent="logout()"
                >登出</router-link
              >
            </div>
          </div>
        </div>
        
        <div v-else class="one">
          <router-link to="/login" class="login-link">登录</router-link>
        </div>
        <div class="two">
          <router-link to="/loging" class="login-link">注册</router-link>
        </div>
      </div>
    </div>
 
  </div>
</template>

<script>
import SearchButton from "@/components/SearchButton.vue";
import authorization from "@/utils/authorization";

export default {
  name: "BlogHeader",
  components: { SearchButton },
  data: function () {
    return {
      username: "",
      hasLogin: false,
      isSuperuser: JSON.parse(localStorage.getItem("isSuperuser.myblog")),
    };
  },
  mounted() {
    authorization().then((data) => ([this.hasLogin, this.username] = data));
  },
  methods: {
    logout() {
      localStorage.clear();
      window.location.reload(false);
    },
    refresh() {
      this.username = localStorage.getItem("username.myblog");
    },
  },
};
</script>

<style scoped>
.dropbtn {
  background-color: rgb(0, 188, 235);
  color: white;
  padding: 8px 8px 30px 8px;
  font-size: 16px;
  border: none;
  cursor: pointer;
  height: 16px;
  border-radius: 5px;
}

/* 容器 <div> - 需要定位下拉内容 */
.dropdown {
  z-index: 1;
  position: absolute;
  top: 10px;
  right: 12px;
  display: inline-block;
}

/* 下拉内容 (默认隐藏) */
.dropdown-content {
  display: none;
  position: absolute;
  background-color: #f9f9f9;
  min-width: 120px;
  box-shadow: 0 8px 16px 0 rgba(0, 0, 0, 0.2);
  text-align: center;
}

/* 下拉菜单的链接 */
.dropdown-content a {
  color: black;
  padding: 12px 16px;
  text-decoration: none;
  display: block;
}

/* 鼠标移上去后修改下拉菜单链接颜色 */
.dropdown-content a:hover {
  background-color: #f1f1f1;
}

/* 在鼠标移上去后显示下拉菜单 */
.dropdown:hover .dropdown-content {
  display: block;
}

/* 当下拉内容显示后修改下拉按钮的背景颜色 */
.dropdown:hover .dropbtn {
  background-color: darkslateblue;
}
</style>

<style scoped>
.login-link {
  color: black;
  text-decoration: none;
}

.login {
  text-align: right;
}

#header {
  text-align: center;
  height: 60px;
}

.grid {
  position: relative;
  display: grid;
  height: 60px;
  grid-template-columns: 1fr 4fr 1fr;
}
.title {
  margin: 0;
  padding: 0;
  height: 60px;
}
.title h1{
    margin: 0;
    padding: 0;
    line-height: 60px;
}

.title button {
  margin-left: 0;
  border: 0;
  background-color: rgba(255, 255, 255, 0);
  font-size: 28px;
}
.one {
  width: 60px;
  height: 60px;
  text-align: center;
  line-height: 60px;
  position: absolute;
  right: 60px;
}
.two {
  width: 60px;
  height: 60px;
  text-align: center;
  line-height: 60px;
  position: absolute;
  right: 0;
}
</style>