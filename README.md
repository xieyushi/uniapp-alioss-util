最近有时间弄了一个阿里云对象存储的小程序直传图片的工具类,用于在uniapp中开发小程序时可以使用.现在也已经提交到uniapp的插件市场,链接是[https://ext.dcloud.net.cn/plugin?id=278](https://ext.dcloud.net.cn/plugin?id=278)  
github上已经上传,链接[https://github.com/xieyushi/uniapp-alioss-util](https://github.com/xieyushi/uniapp-alioss-util)  
<!--more-->uniapp的阿里云OSS上传工具插件使用方法:
将ossutil文件夹放在common文件夹下.
修改config.js中的配置信息:

```
var fileHost = 'https://XXX.XXX.aliyuncs.com/';//你的阿里云地址最后面跟上一个/   在你当前小程序的后台的uploadFile 合法域名也要配上这个域名
var config = {
   //aliyun OSS config
  uploadImageUrl: `${fileHost}`, // 默认存在根目录，可根据需求改
  AccessKeySecret: 'XXXXXXX',        // AccessKeySecret 去你的阿里云上控制台上找
  OSSAccessKeyId: 'XXXXXXX',         // AccessKeyId 去你的阿里云上控制台上找
   timeout: 87600 //这个是上传文件时Policy的失效时间
};
```

页面引入方式为:

```
import uploadImage from '@/common/ossutil/uploadFile.js';
```

页面中上传图片代码:

```
uni.chooseImage({
	count: 1, // 默认最多一次选择9张图
	sourceType: ['camera'], // 可以指定来源是相册还是相机，默认二者都有
	success: res=> {
		// 返回选定照片的本地文件路径列表，tempFilePath可以作为img标签的src属性显示图片
		var tempFilePaths = res.tempFilePaths;

		//支持多图上传
		for (var i = 0; i < res.tempFilePaths.length; i++) {
			//显示消息提示框
			uni.showLoading({
			  mask: true
			})

			//上传图片
			//图片路径可自行修改
			uploadImage(res.tempFilePaths[i], 'images/',
				result => {
				 uni.hideLoading();
			}
			)
		}
	}
})
```
uploadImage方法后的result为图片的oss访问路径.
非常简单,傻瓜式^_^
因为是在小程序中,所以不用担心OSSAccessKeyId和AccessKeySecret会暴露出去,所以也不需要后台来做签名.前端自己来弄就好了.给后端省事,给前端也省事了.