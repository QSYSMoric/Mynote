### 1、概述
+ 本日主要完成登录功能页面架构与登录功能的实现，用到了一些库以便于软件开发，有：
	1. cookie-parser：服务端的cookie管理
	2. js-cookie：客户端对cookie的处理
	3. crypto.createHash：哈希函数的处理
### 2、具体描述
#### 2.1、前端登录实现
+ 建立了基础的表单给客户端提供输入按钮( 我并未做机器人的登录验证，因为目前来说还没有必要 )
```html
<form action="#" class="loginFromContent">
	<div class="loginId">
		<input 
		type="text" 
		placeholder=" Id" 
		v-model.number="userLogin.userID">
	</div>
	<div class="loginPassword">
		<input type="password" 
		autocomplete="off" 
		placeholder=" pas"
		v-model="userLogin.password">
	</div>
	<div class="loginCheckBox">
		<label class="checkbox-container">
			<input type="checkbox">
			<span class="checkmark"></span>隐私通知
		</label>
	</div>
	<div class="loginSubmit">
		<a @click.stop="loginClick">登录</a>
	</div>
</form>
```
+ 其中在setup组件中我使用了
```js
const userLogin = reactive({
	userID:"",
	password:""
});
```
+ 将用户id和用户密码与userLogin对象中的属性相绑定
+ 登录按钮的封装：
```js
import MoricLogin from '../scripts/MoricLogin';
//登录按钮
function loginClick(){
	let loginMsg = {
		userID:userLogin.userID,
		password:userLogin.password
	}
	MoricLogin.loginHandler(loginMsg);
}
```
+ 我将登录功能的具体实现函数封装在一个对象之中，这样符合策略模式，这个按钮只负责逻辑判断，和获取用户输入的信息，并传入，登录函数的具体：
```js
import { useRoute, useRouter } from "vue-router"
import axios from '@/plugins/axiosInstance';
export default {
    router:undefined,
    start(){
        this.router = useRouter();
    },
    //跳转
    jump(){
        this.router.push({
            path:'/MoricHome/MoricHomeUserList',
            name:"MoricHomeUserList",
            query:{
                msg:"hello"
            }
        });
    },
    //跳转到注册部分
    goRegisterContent(){
        this.router.push({
            path:'/MoricRegister',
            name:"MoricRegister",
            query:{
                msg:"hello"
            }
        });
    },
    //登录请求部分
    loginHandler(loginMsg){
        axios({
            url:"/api/login",
            method:"post",
            data:loginMsg,
        }).then((values)=>{
            if(values.data.state){
                alert(values.data.alert);
                this.jump();
            }else{
                alert(values.data.alert);
            }
        });
    }
};
```
+ 我暴露了一个对象，该对象封装了全部所需的功能函数
+ 首先start( );会给我一个路由的具体实例，以便于做路由跳转，然后，将loginHandler函数绑定到登录按钮的click事件中，现在还为做登录字符的限制与登录验证机制，会在以后优化中进行
#### 2.2、后端登录交互实现
+ 在响应客户端发来的请求时，首先会被导航到/api这个专用的接口下
```js
//配置用户路由,处理api中的一系列请求
const routes = require('./server/routers/user');
app.use('/api',routes);
```
+ 其中具体的响应中间路由由其他的子路由帮助我进一步分配
+ 关于路由的分发：
```js
//路由信息的跳转
const express = require('express');
const router = express.Router();
const userController = require('../controllers/userController')
//注册按钮
router.post('/register',userController.create);
//登录按钮
router.post('/login',userController.login);
module.exports = router;
```
+ 每个不同的api接口都是由userController具体的功能函数对象所保存的所以这里只是做的关于对路由的分配功能导航至正确的功能处理程序中
+ 我们在讲关于userController.login对于处理程序之前，先讲讲app中对于cookie-parser库的使用
```js
//配置中间件cookie处理cookie
const secretKey = "Moric";//密钥
const cookieParser=require("cookie-parser");
app.use(cookieParser(secretKey));
```
+ 该库主要是用于设置客户端cookie，cookie对于客户端做一系列操作而言都是至关重要的凭证
+ 然后是对于登录的响应程序讲解：
```js
//登录按钮
exports.login = function(req,res){
    console.log("用户的cookie",req.signedCookies["user_id"]);
    let {userID,password} = req.body;
    console.log(userID);
    connection.query(
        'SELECT * FROM users WHERE user_id = ?',
        [userID],
        (err,rows) => {
			if(!err){
				if(rows.length === 0){
					res.clearCookie();
					res.send({ 
						alert:'没有这个账号',
						state:false,
					});
				}else{
					let userMsg = {};
					({
						user_id:userMsg.user_id,
						username:userMsg.username,
						password:userMsg.password,
						avatar:userMsg.avatar,
						avatar_type:userMsg.avatar_type
					} = rows);
					res.cookie("user_id",userMsg.user_id, {
						//最大失效时间
						maxAge: 1000 * 60 * 60 * 24 * 2,
						//路径
						path: '/',
						signed:true //加密属性
					});
					res.cookie("isLoggedin",true);
					tempUser.saveTempAvatar(userMsg.avatar,String(userMsg.user_id)).then(
						(value)=>{
						userMsg.avatar = value;
						res.send({
							alert:"登录成功",
							state:true,
							userMsg
						});
					},(err)=>{
						console.log("发生错误:" + err);
						res.clearCookie();
						userMsg.avatar = undefined;
						res.send({
							alert:"发生错误",
							state:false,
							userMsg,
						});
					});
				}
			}else{
				console.log(err);
				res.clearCookie();
				res.send({ 
					alert:'没有这个账号' + err,
					state:false,
				});
			}
        }
    );
}
```
+ 首先，查看客户端的cookie是否已经登录，然后通过客户端发送的post请求中的数据得到用户填写的用户id和密码，我再从数据库中根据user_id查找相应的用户信息，导出
+ 判断是否为空，为空代表该用户不存在，再通过导出的密码判断是否与用户输入的相等，再进行下一步
```js
let userMsg = {};
({
	user_id:userMsg.user_id,
	username:userMsg.username,
	password:userMsg.password,
	avatar:userMsg.avatar,
	avatar_type:userMsg.avatar_type
} = rows);
```
+ 通过es6的解构赋值的操作，我将所需的用户信息保存到了userMsg中
```js
res.cookie("user_id",userMsg.user_id, {
	//最大失效时间
	maxAge: 1000 * 60 * 60 * 24 * 2,
	//路径
	path: '/',
	signed:true //加密属性
});
res.cookie("isLoggedin",true);
```
+ 然后为客户端更新对应的cookie
+ 对于用户的头像图片资源
```js
if(userMsg.avatar){
	tempUser.saveTempAvatar(
	userMsg.avatar,String(userMsg.user_id)).then(
		(value)=>{
		userMsg.avatar = value;
		res.send({
			alert:"登录成功",
			state:true,
			userMsg
		});
	},(err)=>{
		console.log("发生错误:" + err);
		res.clearCookie();
		userMsg.avatar = undefined;
		res.send({
			alert:"发生错误",
			state:false,
			userMsg,
		});
	});
}
```
+ 我的打算是将用户的头像临时存储在一个以用户id为名的文件夹中这里存储用户的头像资源，这个功能被我封装到一个对象之中，返回这个头像资源的url地址，传给客户端，客户端到时候再请求便是
```js
const fs = require('fs');
const path = require('path');
const crypto = require('crypto');
/**
 * 从数据库中提取用户头像的 Base64 编码,然后将其保存为临时文件
 * @param {string} base64ImageData 用户头像的 Base64 编码
 * @param {string} username 用户名
 * @returns {Promise<string>} 返回临时文件存储目录的路径
 */
function saveTempAvatar(base64ImageData, username) {
  const imgBuffer = Buffer.from(base64ImageData, 'base64');
  const hash = crypto.createHash('md5').update(username).digest('hex');
  const dirPath = path.join(__dirname, '../' ,'server' ,'temp', hash);
  return new Promise((resolve, reject) => {
    fs.mkdir(dirPath, { recursive: true }, (err) => {
      if (err) {
        console.log(err);
        reject(err);
      } else {
        const filePath = path.join(dirPath, `${username}.png`);
        fs.writeFile(filePath, imgBuffer, (err) => {
          if (err) {
            reject(err);
          } else {
            resolve(hash);
          }
        });
      }
    });
  });
}
```
+ 这样基本的登录功能便实现了，之后会对其进行优化重构