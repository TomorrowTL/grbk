/* 登录组件 */
<template>
<div>
    <BlogHeader/>

    <div id="grid">
        <div id="signin">
            
            <div class="froms">
                <h3>登录账号</h3>
            <form>
                <div class="form-elem">
                    <span>账号：</span>
                    <input v-model="signinName" type="text" placeholder="输入用户名">
                </div>

                <div class="form-elem">
                    <span>密码：</span>
                    <input v-model="signinPwd" type="password" placeholder="输入密码">
                </div>

                <div class="form-elem">
                    <button v-on:click.prevent="signin">登录</button>
                </div>
            </form>
            </div>
        </div>
    </div>
    


    <BlogFooter/>
</div>
    

</template>

<script>
    import axios from 'axios';
    import BlogHeader from '@/components/BlogHeader.vue'
    import BlogFooter from '@/components/BlogFooter.vue'


    export default {
        name: 'Login',
        components: {BlogHeader, BlogFooter},
        data: function () {
            return {
                signupName: '',
                signupPwd: '',
                signinName: '',
                signinPwd: '',
                signupResponse: null,
            }
        },
        methods: {
            signup() {
                const that = this;
                axios
                    .post('/api/user/', {
                        username: this.signupName,
                        password: this.signupPwd,
                    })
                    .then(function (response) {
                        that.signupResponse = response.data;
                        alert('用户注册成功，快去登录吧！');
                    })
                    .catch(function (error) {

                        alert(error.message);

                        // Handling Error here...

                        // https://github.com/axios/axios#handling-errors

                    });
            },
            signin() {
                const that = this;
                axios
                    .post('/api/token/', {
                        username: that.signinName,
                        password: that.signinPwd,
                    })
                    .then(function (response) {
                        const storage = localStorage;
                        // Date.parse(...) 返回1970年1月1日UTC以来的毫秒数
                        const expiredTime = Date.parse(response.headers.date) + 60 * 100 * 1000;
                        // 设置 localStorage
                        storage.setItem('access.myblog', response.data.access);
                        storage.setItem('refresh.myblog', response.data.refresh);
                        storage.setItem('expiredTime.myblog', expiredTime);
                        storage.setItem('username.myblog', that.signinName);

                        // 是否为管理员
                        axios
                            .get('/api/user/' + that.signinName + '/')
                            .then(function (response) {
                                storage.setItem('isSuperuser.myblog', response.data.is_superuser);
                                // 路由跳转
                                that.$router.push({name: 'Home'});
                            });
                            // .catch(...)
                    })
                // .catch(...)
            },
        }
    }
</script>

<style scoped>

    #grid {
        display: grid;
    }



    #signin {
        position: relative;
        top: 100px;
        width: 400px;
        height: 400px;
        display: flex;
        justify-content: center;
        align-content: center;
        margin: auto;
        background-color:rgba(255, 255, 255, 0.1);
        backdrop-filter: blur(5px);
        box-shadow: 0 25px 45px rgba(0, 0, 0, 0.1);
        border: 1px solid rgba(255, 255, 255, 0.5);
         border-radius: 10px;
        border-right: 1px solid rgba(255, 255, 255, 0.1);
        border-bottom: 1px solid rgba(255, 255, 255, 0.5);
        
    }
    #signin h3{
        position: relative;
        color: #fff;
        font-size: 24PX;
        font-weight: 600;
        letter-spacing: 5PX ;
        margin-bottom: 30px;
        cursor: pointer;
        text-align: center;
    }

    .froms{
        position: absolute;
       
        padding:25px 50px;
    }
    .froms input{
        width: 300px;
        height: 40px;
        padding: 10px auto;
        background:rgba(255, 255, 255, 0.3 );
        outline: none;
        border: none;
        border-radius: 30px;
        border:  rgba(255, 255, 255, 0.5);
        border-right:1px solid rgba(255, 255, 255, 0.1 );
        border-bottom: 1px solid rgba(255, 255, 255, 0.1 );
        font-size: 16px;
        letter-spacing: 1px;
        box-shadow: 0 5px 15px rgba(0, 0, 0, 0.05);
    }
    .froms button{
        width: 316px;
        height: 40px;
        background: rgb(14, 172, 211);
        outline: none;
        border: none;
        border-radius: 30px;
        border:  rgba(255, 255, 255, 0.5);
        border-right:1px solid rgba(255, 255, 255, 0.1 );
        border-bottom: 1px solid rgba(255, 255, 255, 0.1 );
        font-size: 16px;
        letter-spacing: 1px;
        box-shadow: 0 5px 15px rgba(0, 0, 0, 0.05);

    }

    .form-elem {
        padding: 10px;
        
        font-weight: 600;
    }



</style>