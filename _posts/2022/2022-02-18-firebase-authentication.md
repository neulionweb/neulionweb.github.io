---
layout: post
title:  "用Firebase Authentication给你的Web app快速的加个验证"
tags: firebase authentication web app
category: dev
---
## 需求
我们需要对一个部署在公网上的demo site做访问限制，只允许特定的人可以访问，所以需要一个简单快速的鉴权方案。

## Authentication
Firebase Authentication ([文档](https://firebase.google.com/docs/auth?hl=zh-cn)) 提供了快捷的登录验证功能。
他向后接入了Google， Facebook，Twitter，GitHub，Apple等多种第三方登录，也支持Email/Password登录，向前提供了API方便App集成。

## 方案
根据需求，我们决定使用Firebase Auth的 [Email/Password登录](https://firebase.google.com/docs/auth/web/password-auth) 功能。
用户在访问Webapp之前，需要用预制的账号密码先登录。

## 用到的库: firebase & firebaseui
* `firebase`这个库提供了API，包括登录登出。  
* `firebaseui`[文档](https://firebase.google.com/docs/auth/web/firebaseui) 提供了一个基础登录界面，整合了API，使用起来更方便。这正是我需要的。  
预制的登录界面如下：  
![Sign In]({{ site.cdn_prefix }}assets/images/2022/02/18/sign-in.png)

## 实现
### 配置
在 [Firebase 控制台](https://console.firebase.google.com/) 中，打开**身份验证**部分并启用电子邮件地址和密码身份验证。

### 依赖
`npm install firebase firebaseui --save`  

### 代码片段
初始化
```
import firebase from 'firebase/compat/app';
import * as firebaseui from 'firebaseui';

// TODO: Replace the following with your app's Firebase project configuration
const firebaseConfig = {
  //...
};

firebase.initializeApp(firebaseConfig);
```
显示Login Form
```
const ui = new firebaseui.auth.AuthUI(firebase.auth());
ui.start('#firebaseui-auth-container', {
    callbacks: {
        signInSuccessWithAuthResult(authResult, redirectUrl) {
            // User successfully signed in.
            // Store a flag to avoid seeing login form when page reload.
            localStorage.setItem('firebaseAuth', '1');
            successCallback();

            // Return type determines whether we continue the redirect automatically
            // or whether we leave that to developer to handle.
            // Here we return false to avoid redirects after sign-in.
            return false;
        },
    },
    signInFlow: 'popup',
    signInOptions: [
        {
            // Only use Email/Password
            provider: firebase.auth.EmailAuthProvider.PROVIDER_ID,
            disableSignUp: {
                status: true,
            },
        },
    ],
});
```
检测用户登录状态
```
firebase.auth().onAuthStateChanged(user => {
    const authenticated = localStorage.getItem('firebaseAuth') !== null;
    // Check firebase user status, see if it is sync with the cache or not
    if (user && !authenticated) {
        localStorage.setItem('firebaseAuth', '1');
        ui.reset();
        successCallback();
    }
    if (!user && authenticated) {
        localStorage.removeItem('firebaseAuth');
        window.location.reload();
    }
});
```

### 完整代码
index.ts
```
import firebase from 'firebase/compat/app';
import * as firebaseui from 'firebaseui';
import './styles.scss';

export const firebaseAuth = app => {
    // Turn off for localhost dev mode
    if (document.domain === 'localhost') {
        app();
        return;
    }

    // TODO: Replace the following with your app's Firebase project configuration
    const firebaseConfig = {
      //...
    };
	firebase.initializeApp(firebaseConfig);
	
	const ui = new firebaseui.auth.AuthUI(firebase.auth());
	
	// Add a simple way to Sign Out Firebase. This is optional.
	const renderSignOut = () => {
		const logoutBtn = document.createElement('a');
		logoutBtn.onclick = () => firebase.auth().signOut();
		const linkText = document.createTextNode('Quit');
		logoutBtn.appendChild(linkText);
		logoutBtn.className = 'gigya-signout';
		logoutBtn.href = 'javascript:void(0);';
		document.getElementsByTagName('body')[0].appendChild(logoutBtn);
	};
	
	const successCallback = () => {
		app();
		renderSignOut(firebase);
	};
	
	// Firebase Container
	const container = document.createElement('DIV');
	container.id = 'firebaseui-auth-container';
	document.getElementsByTagName('body')[0].appendChild(container);

	if (localStorage.getItem('firebaseAuth') !== null) {
		successCallback();
	} else {
		ui.start('#firebaseui-auth-container', {
			callbacks: {
				signInSuccessWithAuthResult(authResult, redirectUrl) {
					// User successfully signed in.
					// Store a flag to avoid seeing login form when page reload.
					localStorage.setItem('firebaseAuth', '1');
					successCallback();

					// Return type determines whether we continue the redirect automatically
					// or whether we leave that to developer to handle.
					// Here we return false to avoid redirects after sign-in.
					return false;
				},
			},
			signInFlow: 'popup',
			// Only use Email/Password
			signInOptions: [
				{
					// Only use Email/Password
					provider: firebase.auth.EmailAuthProvider.PROVIDER_ID,
					disableSignUp: {
						status: true,
					},
				},
			],
		});
	}

	firebase.auth().onAuthStateChanged(user => {
	    const authenticated = localStorage.getItem('firebaseAuth') !== null;
		// Check firebase user status, see if it is sync with the cache or not
		if (user && !authenticated) {
			localStorage.setItem('firebaseAuth', '1');
			ui.reset();
			successCallback();
		}
		if (!user && authenticated) {
			localStorage.removeItem('firebaseAuth');
			window.location.reload();
		}
	});
};
```
styles.scss
```
@import '../../../../node_modules/firebaseui/dist/firebaseui.css';

.gigya-signout {
	color: #fff;
	position: fixed;
	right: 0;
	bottom: 0;
}
```
app.tsx
```
import { firebaseAuth } from '~components/firebase';

const init = async () => {
    // Init Web App
};

firebaseAuth(init);
```

### 注意
* firebase 目前最新的是v9，是在2021/8发布的。因为比较新，所以网上的代码示例需要注意他的版本。  
* 目前最新的 firebaseui(v6.0.0) 并不完全支持v9，只能跑在v9的 [Compat](https://firebase.google.com/docs/web/modular-upgrade?hl=zh-cn#about_the_compat_libraries) 模式。这一点，官方文档里有提到：
> 注意：FirebaseUI 与 v9 模块化 SDK 不兼容。借助 v9 兼容性层（具体来说，是 app-compat 和 auth-compat 软件包），您可以将 FirebaseUI 与 v9 搭配使用，但无法缩减应用大小，也无法享用 v9 SDK 的其他优势。

所以官方示例代码需要参考`Web 版本 8`。
* 官网的 firebaseui 的 [demo](https://firebase.google.com/docs/auth/web/firebaseui) 并没有直接提供`module import`的示例代码，所以并不能简单的复制黏贴。

## 最后
这只是Firebase Authentication的一个极其简单的应用场景。他的完整功能比这个多得多。  
如果你需要在app里完全用Firebase做用户模块，也可以看下 `react-firebaseui`<https://github.com/firebase/firebaseui-web-react>。

## 最后的最后
功能写完交付后了，才发现有个漏洞：Firebase不能完全关闭新用户注册……，
顶多只能通过`signInOptions.disableSignUp`把UI上的注册入口关闭了，但在SDK层面上仍然可以注册新用户。
完全关闭注册的开关在Google Cloud Identity Platform里，详见[Firebase文档说明](https://github.com/firebase/firebaseui-web#configure-email-provider)。
具体位置见下图：  
![Google Identity Platform]({{ site.cdn_prefix }}assets/images/2022/02/18/google-identity.png)
而进到这里，你得首先启用Google Cloud Service，然后启用Identity Platform并升级你的App(会提示你此操作不可逆)……
好在看了下Pricing，小范围使用应该不会收费。这样一个简单的功能，并没在Firebase Console里提供，有点坑。  
好吧就先这样吧ㄱ. г
