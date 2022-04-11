### IOS保存到桌面，全屏教程

```html
1.在index.html的头部添加下面几个标签（可以在js动态加载）
<meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0,viewport-fit=cover">
<meta name="apple-touch-fullscreen" content="yes">
<meta name="apple-mobile-web-app-capable" content="yes" />
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent" />
<link rel="apple-touch-icon" href="./static/icons/logo.png" />

第一个标签：
intial-scale:页面首次被显示是可视区域的缩放级别，取值1.0则页面按实际尺寸显示，无任何缩放
maximum-scale=1.0, minimum-scale=1.0;可视区域的缩放级别，
maximum-scale用户可将页面放大的程序，1.0将禁止用户放大到实际尺寸之上。
user-scalable:是否可对页面进行缩放，no 禁止缩放
viewport-fit=cover:使页面占满整个屏幕
第二个标签：
apple-touch-fullscreen: ios添加到主屏幕后，是否全屏显示。
第三个标签：
apple-mobile-web-app-capable：删除默认的苹果工具栏和菜单栏。content有两个值”yes”和”no”,当我们需要显示工具栏和菜单栏时，这个行meta就不用加了，默认就是显示
第四个标签：
apple-mobile-web-app-status-bar-style：状态栏颜色，默认值为default（白色），可以定为black（黑色）和black-translucent（灰色半透明）
第五个标签：
保存到手机主屏幕的logo
```



```
2.在index.html的头部添加各个尺寸的启动图（可以在js动态加载）
<link rel="apple-touch-startup-image" href="apple-splash-2048-2732.jpg" media="(device-width: 1024px) and (device-height: 1366px) and (-webkit-device-pixel-ratio: 2) and (orientation: portrait)"/>
		<link rel="apple-touch-startup-image" href="apple-splash-1668-2388.jpg" media="(device-width: 834px) and (device-height: 1194px) and (-webkit-device-pixel-ratio: 2) and (orientation: portrait)"/>
		<link rel="apple-touch-startup-image" href="apple-splash-1536-2048.jpg" media="(device-width: 768px) and (device-height: 1024px) and (-webkit-device-pixel-ratio: 2) and (orientation: portrait)"/>
		<link rel="apple-touch-startup-image" href="apple-splash-1668-2224.jpg" media="(device-width: 834px) and (device-height: 1112px) and (-webkit-device-pixel-ratio: 2) and (orientation: portrait)"/>
		<link rel="apple-touch-startup-image" href="apple-splash-1620-2160.jpg" media="(device-width: 810px) and (device-height: 1080px) and (-webkit-device-pixel-ratio: 2) and (orientation: portrait)"/>
		<link rel="apple-touch-startup-image" href="apple-splash-1284-2778.jpg" media="(device-width: 428px) and (device-height: 926px) and (-webkit-device-pixel-ratio: 3) and (orientation: portrait)"/>
		<link rel="apple-touch-startup-image" href="apple-splash-1170-2532.jpg" media="(device-width: 390px) and (device-height: 844px) and (-webkit-device-pixel-ratio: 3) and (orientation: portrait)"/>
		<link rel="apple-touch-startup-image" href="apple-splash-1125-2436.jpg" media="(device-width: 375px) and (device-height: 812px) and (-webkit-device-pixel-ratio: 3) and (orientation: portrait)"/>
		<link rel="apple-touch-startup-image" href="apple-splash-1242-2688.jpg" media="(device-width: 414px) and (device-height: 896px) and (-webkit-device-pixel-ratio: 3) and (orientation: portrait)"/>
		<link rel="apple-touch-startup-image" href="apple-splash-828-1792.jpg" media="(device-width: 414px) and (device-height: 896px) and (-webkit-device-pixel-ratio: 2) and (orientation: portrait)"/>
		<link rel="apple-touch-startup-image" href="apple-splash-1242-2208.jpg" media="(device-width: 414px) and (device-height: 736px) and (-webkit-device-pixel-ratio: 3) and (orientation: portrait)"/>
		<link rel="apple-touch-startup-image" href="apple-splash-750-1334.jpg" media="(device-width: 375px) and (device-height: 667px) and (-webkit-device-pixel-ratio: 2) and (orientation: portrait)"/>
		<link rel="apple-touch-startup-image" href="apple-splash-640-1136.jpg" media="(device-width: 320px) and (device-height: 568px) and (-webkit-device-pixel-ratio: 2) and (orientation: portrait)"/>
```



```
3.在index.html添加清单列表文件（可以在js动态加载）
<link rel="manifest" href="./manifest.json" />

manifest.json的内容如下
{
	"name":"APP名称",
	"theme_color":"#FEFEFE",
	"display":"standalone",
	"background_color":"#FFFFFF",
	"start_url": "/",
	"orientation":"portrait",
	"icons": [
	    {
	      "src": "/static/h5/icons/apple-touch-icon-152x152.png",
	      "sizes": "152x152",
	      "type": "image/png"
	    },
	    {
	      "src": "/static/h5/icons/apple-touch-icon-180x180.png",
	      "sizes": "180x180",
	      "type": "image/png"
	    },
	    {
	      "src": "/static/h5/icons/android-chrome-192x192.png",
	      "sizes": "192x192",
	      "type": "image/png"
	    },
	    {
	      "src": "/static/h5/icons/android-chrome-512x512.png",
	      "sizes": "512x512",
	      "type": "image/png"
	    },
	    {
	      "src": "/static/h5/icons/android-chrome-maskable-192x192.png",
	      "sizes": "192x192",
	      "type": "image/png",
	      "purpose": "maskable"
	    },
	    {
	      "src": "/static/h5/icons/android-chrome-maskable-512x512.png",
	      "sizes": "512x512",
	      "type": "image/png",
	      "purpose": "maskable"
	    }
	  ],
	  "bokkmarksIcon":"/static/h5/icons/logo.png",
	  "appleSplash":{
		  "apple-splash-2048-2732.jpg":"/static/h5/icons/apple-splash-images/apple-splash-2048-2732.jpg",
		  "apple-splash-1668-2388.jpg":"/static/h5/icons/apple-splash-images/apple-splash-1668-2388.jpg",
		  "apple-splash-1536-2048.jpg":"/static/h5/icons/apple-splash-images/apple-splash-1536-2048.jpg",
		  "apple-splash-1668-2224.jpg":"/static/h5/icons/apple-splash-images/apple-splash-1668-2224.jpg",
		  "apple-splash-1620-2160.jpg":"/static/h5/icons/apple-splash-images/apple-splash-1620-2160.jpg",
		  "apple-splash-1284-2778.jpg":"/static/h5/icons/apple-splash-images/apple-splash-1284-2778.jpg",
		  "apple-splash-1170-2532.jpg":"/static/h5/icons/apple-splash-images/apple-splash-1170-2532.jpg",
		  "apple-splash-1125-2436.jpg":"/static/h5/icons/apple-splash-images/apple-splash-1125-2436.jpg",
		  "apple-splash-1242-2688.jpg":"/static/h5/icons/apple-splash-images/apple-splash-1242-2688.jpg",
		  "apple-splash-828-1792.jpg":"/static/h5/icons/apple-splash-images/apple-splash-828-1792.jpg",
		  "apple-splash-1242-2208.jpg":"/static/h5/icons/apple-splash-images/apple-splash-1242-2208.jpg",
		  "apple-splash-750-1334.jpg":"/static/h5/icons/apple-splash-images/apple-splash-750-1334.jpg",
		  "apple-splash-640-1136.jpg":"/static/h5/icons/apple-splash-images/apple-splash-640-1136.jpg"
	  }
}

```



#### index最终代码如下（以uniapp的index.html为例，图片地址自己修改）：

```
<!DOCTYPE html>
<html lang="zh-CN">
    <head>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
		<meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0, viewport-fit=cover">
		<meta name="apple-touch-fullscreen" content="yes">
		<meta name="apple-mobile-web-app-capable" content="yes" />
		<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent" />
		<!-- 清单文件（可以在js动态加载） -->
		<link rel="manifest" href="/static/manifest/manifest.json" />
		<!-- logo （可以在js动态加载）-->
		<link rel="apple-touch-icon" href="./static/manifest/icons/logo.png" />
		<link rel="apple-touch-icon" sizes="152x152" href="touch-icon-152x152.png" /> 
		<link rel="apple-touch-icon" sizes="180x180" href="touch-icon-180x180.png" /> 
		<link rel="apple-touch-icon" sizes="192x192" href="touch-icon-192x192.png" /> 
		
		<!-- 启动图 （可以在js动态加载）-->
		<link rel="apple-touch-startup-image" href="apple-splash-2048-2732.jpg" media="(device-width: 1024px) and (device-height: 1366px) and (-webkit-device-pixel-ratio: 2) and (orientation: portrait)"/>
		<link rel="apple-touch-startup-image" href="apple-splash-1668-2388.jpg" media="(device-width: 834px) and (device-height: 1194px) and (-webkit-device-pixel-ratio: 2) and (orientation: portrait)"/>
		<link rel="apple-touch-startup-image" href="apple-splash-1536-2048.jpg" media="(device-width: 768px) and (device-height: 1024px) and (-webkit-device-pixel-ratio: 2) and (orientation: portrait)"/>
		<link rel="apple-touch-startup-image" href="apple-splash-1668-2224.jpg" media="(device-width: 834px) and (device-height: 1112px) and (-webkit-device-pixel-ratio: 2) and (orientation: portrait)"/>
		<link rel="apple-touch-startup-image" href="apple-splash-1620-2160.jpg" media="(device-width: 810px) and (device-height: 1080px) and (-webkit-device-pixel-ratio: 2) and (orientation: portrait)"/>
		<link rel="apple-touch-startup-image" href="apple-splash-1284-2778.jpg" media="(device-width: 428px) and (device-height: 926px) and (-webkit-device-pixel-ratio: 3) and (orientation: portrait)"/>
		<link rel="apple-touch-startup-image" href="apple-splash-1170-2532.jpg" media="(device-width: 390px) and (device-height: 844px) and (-webkit-device-pixel-ratio: 3) and (orientation: portrait)"/>
		<link rel="apple-touch-startup-image" href="apple-splash-1125-2436.jpg" media="(device-width: 375px) and (device-height: 812px) and (-webkit-device-pixel-ratio: 3) and (orientation: portrait)"/>
		<link rel="apple-touch-startup-image" href="apple-splash-1242-2688.jpg" media="(device-width: 414px) and (device-height: 896px) and (-webkit-device-pixel-ratio: 3) and (orientation: portrait)"/>
		<link rel="apple-touch-startup-image" href="apple-splash-828-1792.jpg" media="(device-width: 414px) and (device-height: 896px) and (-webkit-device-pixel-ratio: 2) and (orientation: portrait)"/>
		<link rel="apple-touch-startup-image" href="apple-splash-1242-2208.jpg" media="(device-width: 414px) and (device-height: 736px) and (-webkit-device-pixel-ratio: 3) and (orientation: portrait)"/>
		<link rel="apple-touch-startup-image" href="apple-splash-750-1334.jpg" media="(device-width: 375px) and (device-height: 667px) and (-webkit-device-pixel-ratio: 2) and (orientation: portrait)"/>
		<link rel="apple-touch-startup-image" href="apple-splash-640-1136.jpg" media="(device-width: 320px) and (device-height: 568px) and (-webkit-device-pixel-ratio: 2) and (orientation: portrait)"/>
		
		
        <title>
            <%= htmlWebpackPlugin.options.title %>
        </title>
        <script>
            var coverSupport = 'CSS' in window && typeof CSS.supports === 'function' && (CSS.supports('top: env(a)') || CSS.supports('top: constant(a)'))
            document.write('<meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0' + (coverSupport ? ', viewport-fit=cover' : '') + '" />')
        </script>
		<link href="./static/logo.png"/>
        <link rel="stylesheet" href="<%= BASE_URL %>static/index.<%= VUE_APP_INDEX_CSS_HASH %>.css" />
		
		
    </head>
    <body>
        <noscript>
            <strong>Please enable JavaScript to continue.</strong>
        </noscript>
        <div id="app"></div>
        <!-- built files will be auto injected -->
    </body>
</html><!DOCTYPE html>
<html lang="zh-CN">
    <head>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <title>
            <%= htmlWebpackPlugin.options.title %>
        </title>
        <script>
            var coverSupport = 'CSS' in window && typeof CSS.supports === 'function' && (CSS.supports('top: env(a)') || CSS.supports('top: constant(a)'))
            document.write('<meta name="viewport" content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0, minimum-scale=1.0' + (coverSupport ? ', viewport-fit=cover' : '') + '" />')
        </script>
        <link rel="stylesheet" href="<%= BASE_URL %>static/index.<%= VUE_APP_INDEX_CSS_HASH %>.css" />
    </head>
    <body>
        <noscript>
            <strong>Please enable JavaScript to continue.</strong>
        </noscript>
        <div id="app"></div>
        <!-- built files will be auto injected -->
    </body>
</html>
```

#### manifest.json文件（图片地址自己修改）：

```
{
	"name":"APP名称",
	"theme_color":"#FEFEFE",
	"display":"standalone",
	"background_color":"#FFFFFF",
	"start_url": "/",
	"orientation":"portrait",
	"icons": [
	    {
	      "src": "/static/h5/icons/apple-touch-icon-152x152.png",
	      "sizes": "152x152",
	      "type": "image/png"
	    },
	    {
	      "src": "/static/h5/icons/apple-touch-icon-180x180.png",
	      "sizes": "180x180",
	      "type": "image/png"
	    },
	    {
	      "src": "/static/h5/icons/android-chrome-192x192.png",
	      "sizes": "192x192",
	      "type": "image/png"
	    },
	    {
	      "src": "/static/h5/icons/android-chrome-512x512.png",
	      "sizes": "512x512",
	      "type": "image/png"
	    },
	    {
	      "src": "/static/h5/icons/android-chrome-maskable-192x192.png",
	      "sizes": "192x192",
	      "type": "image/png",
	      "purpose": "maskable"
	    },
	    {
	      "src": "/static/h5/icons/android-chrome-maskable-512x512.png",
	      "sizes": "512x512",
	      "type": "image/png",
	      "purpose": "maskable"
	    }
	  ],
	  "bokkmarksIcon":"/static/h5/icons/logo.png",
	  "appleSplash":{
		  "apple-splash-2048-2732.jpg":"/static/h5/icons/apple-splash-images/apple-splash-2048-2732.jpg",
		  "apple-splash-1668-2388.jpg":"/static/h5/icons/apple-splash-images/apple-splash-1668-2388.jpg",
		  "apple-splash-1536-2048.jpg":"/static/h5/icons/apple-splash-images/apple-splash-1536-2048.jpg",
		  "apple-splash-1668-2224.jpg":"/static/h5/icons/apple-splash-images/apple-splash-1668-2224.jpg",
		  "apple-splash-1620-2160.jpg":"/static/h5/icons/apple-splash-images/apple-splash-1620-2160.jpg",
		  "apple-splash-1284-2778.jpg":"/static/h5/icons/apple-splash-images/apple-splash-1284-2778.jpg",
		  "apple-splash-1170-2532.jpg":"/static/h5/icons/apple-splash-images/apple-splash-1170-2532.jpg",
		  "apple-splash-1125-2436.jpg":"/static/h5/icons/apple-splash-images/apple-splash-1125-2436.jpg",
		  "apple-splash-1242-2688.jpg":"/static/h5/icons/apple-splash-images/apple-splash-1242-2688.jpg",
		  "apple-splash-828-1792.jpg":"/static/h5/icons/apple-splash-images/apple-splash-828-1792.jpg",
		  "apple-splash-1242-2208.jpg":"/static/h5/icons/apple-splash-images/apple-splash-1242-2208.jpg",
		  "apple-splash-750-1334.jpg":"/static/h5/icons/apple-splash-images/apple-splash-750-1334.jpg",
		  "apple-splash-640-1136.jpg":"/static/h5/icons/apple-splash-images/apple-splash-640-1136.jpg"
	  }
}

```

