# 要买车用户中心公用js库.
用户中心文档

- [API](#API)
  - [`<getSMSCode>`](#getSMSCode)
  - [`<getVoiceCode>`](#getVoiceCode)
  - [`<register>`](#register)
  - [`<login>`](#login)
  - [`<setPassword>`](#setPassword)
  - [`<changePassword>`](#changePassword)
  - [`<amendPassword>`](#amendPassword)
  - [`<logout>`](#logout)
  - [`<newAccessToken>`](#newAccessToken)
  - [`<getAuthInfo>`](#getAuthInfo)
  - [`<updateAuthInfo>`](#updateAuthInfo)
  - [`<getTakeaAddress>`](#getTakeaAddress)
  - [`<getTakeaAddressById>`](#getTakeaAddressById)
  - [`<addTakeAddress>`](#addTakeAddress)
  - [`<editTakeAddress>`](#editTakeAddress)
  - [`<deleteAddress>`](#deleteAddress)
  - [`<setAddressDefault>`](#setAddressDefault)
  - [`<getCustomerInfo>`](#getCustomerInfo)
  - [`<addCustomer>`](#addCustomer)
  - [`<editCustomer>`](#editCustomer)
  - [`<deleteCustomer>`](#deleteCustomer)
  - [`<uploadPicture>`](#uploadPicture)
 
## 调用方式  

    npm install ymc-uc
    调用需配置环境，如不传env参数则默认dev环境 env: dev , gqc , prd

    AMD调用方式
    var userCenter = request('ymc-uc')
    var uc = new userCenter(env)
    
    内联调用
    <script src="ymc-uc/dist/uc.min.js"></script>
    var uc = new userCenter(env)

## 服务地址
- app、H5等线上需要从外网访问的，请使用如下地址：
	1. DEV: uc.private.yaomaiche.com:7070/uc/cgi
	2. GQC: uc.private.yaomaiche.com:8080/uc/cgi
	3. PROD: api.yaomaiche.com/uc/cgi

- 线上是从内网Web或.NET应用访问，请使用如下地址：
	1. DEV: uc.private.yaomaiche.com:7070/uc/api
	2. GQC: uc.private.yaomaiche.com:8080/uc/api
	3. PROD: api.yaomaiche.com/uc/api
	
已做环境判断，直接调用即可。注：url地址有7070使用的是dev环境API，8080使用的是gqc环境API, m.yaomaiche.com使用的是prd环境API。
	
## API

##### `<getSMSCode>`

发送短信验证码，目前发送验证码有3个场景，注册、登录、忘记密码。

```js
RequestBody:

{
    "mobile":"手机号，不能为空",
    "useCase":"使用场景，枚举：register(注册)/access(登录)/registerOrAccess(注册或登录)/resetPassword(忘记密码)",
    "strictlyCheck":"true/false，是否校验手机号，如果校验，则注册时用户已存在会抛出异常；登录/修改密码时，用户不存在会抛出异常"
}

例：

uc.getSMSCode({
	mobile: '15800924615',
	useCase: 'registerOrAccess',
	strictlyCheck: true,
	success: function(data) {
		console.log(data)
	},
	error: function(error) {
		console.log(error)
	}
});

```

##### `<getVoiceCode>`

发送语音验证码。

```js
RequestBody:

{
    "mobile":"手机号，不能为空",
    "useCase":"使用场景，枚举：register(注册)/access(登录)/registerOrAccess(注册或登录)/resetPassword(忘记密码)",
    "strictlyCheck":"true/false，是否校验手机号，如果校验，则注册时用户已存在会抛出异常；登录/修改密码时，用户不存在会抛出异常"
}

例：

uc.getVoiceCode({
	mobile: '15800924615',
	useCase: 'registerOrAccess',
	strictlyCheck: true,
	success: function(data) {
		console.log(data)
	},
	error: function(error) {
		console.log(error)
	}
});

```

##### `<register>`

用户注册或验证码登录。

```js
RequestBody:

{
    "mobile":"手机号，不能为空",
    "code":"验证码",
    "useCase":"使用场景，枚举：register(注册)/access(登录)/registerOrAccess(注册或登录)/resetPassword(忘记密码)"
}

例：

uc.register({
	mobile: '15800924615',
	useCase: 'registerOrAccess',
	code: '123456',
	success: function(data) {
		console.log(data)
	},
	error: function(error) {
		console.log(error)
	}
});

```

##### `<login>`

用户密码登录.

```js
RequestBody:

{
    "loginName":"登录名",
    "password":"密码"
}

例：

uc.login({
	loginName: '158009267156',
	password: '*****',
	success: function(data) {
		console.log(data);
		alert('登录成功');
		window.location.reload();
	},
	error: function(error,xhr) {
		console.log(xhr)
	}
});

```

##### `<setPassword>`

在注册或者短信验证码登录成功后，返回的数据有一个extra字段，extra用于告知客户端用户的状态，如果返回setPassword，说明该用户没有设置密码，业务端自行决定是否跳转到设置密码页面，如果返回access，正常登录。如果客户端需要进行密码设置，使用该API设置.

```js
RequestBody:

{
    "newPassword":"密码"
}

例：

uc.setPassword({
	newPassword: userPasswordVul,
	success: function(data) {
		console.log(data);
	}
});

```

##### `<changePassword>`

忘记密码后，设置密码，与setPassword使用场景不同，此接口的accessToken需要专门获取，不同于登录的accessToken，注意区别。用于忘记密码后的设置密码。
```js
RequestBody:

{
    "newPassword":"密码"
}

例：

uc.changePassword({
	newPassword: userPasswordVul,
	success: function(data) {
		console.log(data);
	}
});

```

##### `<amendPassword>`

修改密码。
```js
RequestBody:

{
    "password":"旧密码",
    "newPassword":"新密码"
}

例：

uc.amendPassword({
	newPassword: userPasswordVul,
	password: userPasswordVul,
	success: function(data) {
		console.log(data);
	}
});

```

##### `<logout>`

退出登录。

```js
例：

uc.logout({
	success: function() {
		alert('退出成功');
		window.location.reload();
	}
})

```

##### `<newAccessToken>`

刷新accessToken，客户端可以根据过期时间决定何时刷新accessToken，即使accessToken已过期，依然可以进行刷新。刷新后原accessToken将失效。每次刷新获得的新accessToken有效期1天。

```js
例：

uc.newAccessToken({
	success: function(data) {
		console.log(data);
		alert('刷新成功');
		window.location.reload();
	}
})

```

##### `<getAuthInfo>`

获取用户信息.

```js
例：

uc.getAuthInfo({
	success: function(auth) {
		console.log(auth);
	}
});

```

##### `<updateAuthInfo>`

更新用户信息.

```js
RequestBody:
{
    "name":"真实姓名",
    "gender":"性别，男/女",
    "birthday":"生日",
    "headPortrait":"头像",
    "nickName":"昵称",
    "idCard":"省份证"
}

例：

uc.updateAuthInfo({
	"name": userNameVul,
	"gender": userGenderVul,
	"birthday": userBirthdayVul,
	"nickName": userNickNameVul,
	"idCard":userIdCardVul,
	success: function(data) {
		console.log(data);
	}
})

```

##### `<getTakeaAddress>`

查询收货地址.

```js
RequestBody:
{
    
}

例：

uc.getTakeaAddress({
	success: function(auth) {
		console.log(auth);
	}
});	

```

##### `<addTakeAddress>`

#####  `<getTakeaAddressById>`

查询单个收货地址.

```js
RequestBody:
{
    addrId：1
}

例：

uc.getTakeaAddressById({
  addrId:el,
	success: function(auth) {
		console.log(auth);
	}
});

```

##### `<getTakeaAddressById>`

添加收货地址.

```js
RequestBody:
{
    "receiverName":"收件人姓名",
    "telephone":"收件人电话",
    "provinceId":"省Id",
    "cityId":"市Id",
    "districtId":"区Id",
    "provinceCityDistrict":"省市区文字，如：上海 上海市 浦东新区",
    "detailAddress":"详细街道门牌号，如：世纪大道500号",
    "zipCode":"邮编",
    "isDefault":"是否是默认地址"
}

例：

uc.getTakeaAddress({
	"receiverName": receiverNameVul,
	"telephone": userPhoneVul,
	"provinceId": provinceIdVul,
	"cityId": cityIdVul,
	"districtId":districtIdVul,
	"provinceCityDistrict": provinceCityDistrictVul,
	"detailAddress": detailAddressVul,
	"zipCode": zipCodeVul,
	"isDefault": "true",
	success: function(data) {
		console.log(data);
	}
});	

```

##### `<editTakeAddress>`

编辑收货地址.

```js
RequestBody:
{
    "receiverName":"收件人姓名",
    "telephone":"收件人电话",
    "provinceId":"省Id",
    "cityId":"市Id",
    "districtId":"区Id",
    "provinceCityDistrict":"省市区文字，如：上海 上海市 浦东新区",
    "detailAddress":"详细街道门牌号，如：世纪大道500号",
    "zipCode":"邮编",
    "isDefault":"是否是默认地址"
}

例：

uc.editTakeAddress({
	"receiverName": receiverNameVal,
	"telephone": userPhoneVal,
	"provinceId": provinceIdVal,
	"cityId": cityIdVal,
	"districtId":districtIdVal,
	"provinceCityDistrict": provinceCityDistrictVal,
	"detailAddress": detailAddressVal,
	"zipCode": zipCodeVal,
	"isDefault": isDefaultVal,
	"addrId": addid,
	success: function(data) {
		console.log(data);
		addressPopup.style.display = 'none';
		window.location.reload();
	}
});	

```

##### `<deleteAddress>`

删除收货地址.

```js
例：

uc.deleteAddress({
	"addrId": addid, // 地址id
	success: function(data) {
		alert('删除成功');
		window.location.reload();
	}
});	
```

##### `<setAddressDefault>`

设置为默认收货地址.

```js
例：

uc.setAddressDefault({
	"addrId": addid, // 地址id
	success: function(data) {
		alert('设置成功');
		window.location.reload();
	}
});	
```

##### `<getCustomerInfo>`

查询客户信息(购买人，不一定是平台用户).

```js
例：

uc.getCustomerInfo({
	success: function(auth) {
		console.log(auth);
	}
});	
```

##### `<addCustomer>`

添加客户信息.

```js
RequestBody:

{
    "name":"客户姓名",
    "telephone":"联系电话",
    "gender":"性别",
    "idCard":"身份证号"
}

例：

uc.addCustomerInfo({
	"name": customerNameVul,
	"telephone": customerPhoneVul,
	"gender": customerGenderVul,
	"idCard": customerIdCardVul,
	success: function(data) {
		console.log(data);
	}
});	
```
##### `<editCustomer>`

编辑客户信息.

```js
RequestBody:

{
    "name":"客户姓名",
    "telephone":"联系电话",
    "gender":"性别",
    "idCard":"身份证号",
		"cid": "客户ID"
}

例：

uc.editCustomer({
	"name": customerNameVal,
	"telephone": customerPhoneVal,
	"gender": customerGenderVal,
	"idCard": customerIdCardVal,
	"cid": cid,
	success: function(data) {
		console.log(data);
		CustomerPopup.style.display = 'none';
		window.location.reload();
	}
})
```

##### `<deleteCustomer>`

删除客户信息.

```js
例：

uc.deleteCustomer({
	"cid": cid, // 地址id
	success: function(data) {
		alert('删除成功');
		window.location.reload();
	}
});	
```

##### `<uploadPicture>`

上传用户头像.

	<div class="contenior">
		<div class="header">上传用户头像</div>
		<div class="weui-calls">
			<div class="a-upload" id="fileUpload">
				<input id="userPicture" type="file" accept="image/png,image/jpg" />
			</div> 		
		</div>

		<div class="button-wrap">
			<a href="javascript:;" id="uploadUserPicture" class="weui-btn weui-btn_primary">上传头像</a>
		</div>

	</div>
```js
(function() {
	var userPicture = document.getElementById('userPicture');
	var uploadUserPicture = document.getElementById('uploadUserPicture');
	var fileUpload = document.getElementById('fileUpload');
	userPicture.addEventListener('change', function() {
		var that = this;
		uc.uploadPicture({
			formData: that.files[0],
			success: function(data) {
				var url = JSON.parse(data).data.headPortrait;
				fileUpload.style.backgroundImage = 'url("'+url+'")';
				fileUpload.style.backgroundPosition = 'center';
				fileUpload.style.backgroundSize = '100%';
				fileUpload.style.backgroundRepeat = 'no-repeat';
				console.log(url);
			}
		});
	});
}())
```