<template>
 <div class="main">
    <div class="login_wrap">
        <div>
            <el-form
                ref="formRef"
                :model="loginData"
                label-width="100px"
                class="demo-dynamic"
            >
                <el-form-item
                    prop="username"
                    label="用户名"
                    :rules="[
                        {
                        required: true,
                        message: '此项为必填项',
                        trigger: 'blur',
                        }, 
                    ]"
                >
                    <el-input v-model="loginData.username" />
                </el-form-item>
                
                <el-form-item
                    prop="password"
                    label="密码"
                    :rules="[
                    {
                    required: true,
                    message: '此项为必填项',
                    trigger: 'blur',
                    }, 
                ]"
                >
                    <el-input type="password" v-model="loginData.password" />
                </el-form-item>
            </el-form>
             <el-button type="primary" class="login_btn" @click="handleLogin">登录</el-button>
        </div>
    </div>
    </div>
</template>

<script>
import{ useStore }from "vuex"
import { reactive, toRefs } from 'vue'
import { useRouter } from "vue-router"
import {loginApi}from "@/util/request"
    export default {
        name:"login",
        setup(){

        const router = useRouter()
        const store =useStore()
        const count=store.state.number.count
            const data=reactive({
                  loginData:{
                    username:"",
                    password:""
                  },
                  num:count,

            })
            const handleLogin=()=>{
                //请求后台接口
                // 默认用户：admin/123456
                loginApi(data.loginData).then(res=>{
                   if(res.data){
                      store.commit('setUserInfo',res.data);
                      localStorage.setItem("loginData",JSON.stringify(res.data))
                     // 跳转 /user
                    router.push({
                    path:"/"
                    })
                }
            })
           
        } 

            return{
                ...toRefs(data),
                handleLogin
            }
        }
    }
</script>

<style scoped>
     .main{
        width: 100%;
        height: 100vh;
        background: rgb(56, 86, 139);
        position: relative;
     }
    .login_wrap{
    background: center;
	background-color: rgb(255, 255, 255);
	width: 350px;
	height: 200px;
	margin: auto;
	position: absolute;
	top:50%;
    left: 50%;
    transform: translate(-50%,-50%);
    }
    .login_btn{
        display: block;
        margin: 10px auto;
    }
</style>