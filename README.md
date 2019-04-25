# browser-extension-vuejs-study

vuejs 를 이용해서 브라우저 extension을 만드는 방법에 대해서 정리 합니다.

## chrome extension developement
- Tutorial: https://developer.chrome.com/extensions/getstarted
- API: https://developer.chrome.com/extensions/api_index
- Example Code: https://github.com/orbitbot/chrome-extensions-examples

## browser extension boilerplate 참고:
- https://github.com/Kocal/vue-web-extension (recommend)
- https://github.com/YuraDev/vue-chrome-extension-template
- 가장 많이 사용하는 2개의 extension library
- Kocal/vue-web-extension을 추천 하는 이유: yura 빌드해서 chrome://extensions 에서 로드 해보면 에러나서... ^^ 

## Chrome app store에 배포하기
- https://support.google.com/chrome/a/answer/2714278?hl=ko
- https://joshuajangblog.wordpress.com/2016/08/17/chrome-extension-program-development-overview/
- !주의! 개발자 계정 $5 비용 듬

## script 주입하기 (Content Scripts)
- https://developer.chrome.com/extensions/content_scripts
- content script는 페이지와 다른 영역에서 실행됨. tab에 실행된 window 또는 다른 content script는 격리(isolated) 되어 있음
- 2가지 방법으로 script를 웹페이지에 주입. (programmatically or declaratively)


```
// contentScript.js
console.log('hello');
```

Programmatically. (popup.js or messaging)
```
// permissions: ['activeTab'] 필요. (manifest.json)
chrome.tabs.executeScript({
  file: 'contentScript.js'
});
```

Declaratively. (manifest.json)
```
"content_scripts": [
    {
      "matches": ["https://www.google.com/*"],
      "js": ["contentScript.js"]
    }
  ]
```

## Multiple Entry
 - glob entries plugin 사용
 - watch 이슈로 webpack-watched-glob-entires-plugin 사용.
 
   https://github.com/Milanzor/webpack-watched-glob-entries-plugin )
 - entry 디렉토리 경로를 그대로 output으로 사용하려면 glob primer의 @() 을 이용하여 base: '.'로 인지 되도록 처리
 
   https://github.com/isaacs/node-glob, https://www.npmjs.com/package/glob-base
 
설치
```
npm install --save-dev webpack-watched-glob-entries-plugin
```

webpack.config.js
```
  entry: WebpackWatchedGlobEntries.getEntries(
    [ 
      path.resolve(__dirname + '/src/', 'background.js'),
      path.resolve(__dirname + '/src/', '@(popup)/*.js'),
      path.resolve(__dirname + '/src/', '@(content-scripts)/**/*.js'),
      // path.resolve(__dirname, 'src/**/*.js'),
    ]
  ),
```

output
```
               background.js   78.5 KiB       0  [emitted]  background
  content-scripts/content.js  954 bytes       1  [emitted]  content-scripts/content
content-scripts/test/test.js  934 bytes       2  [emitted]  content-scripts/test/test
          icons/icon_128.png   9.81 KiB          [emitted]  
           icons/icon_48.png   2.52 KiB          [emitted]  
               manifest.json  551 bytes          [emitted]  
             popup/popup.css   43 bytes       3  [emitted]  popup/popup
            popup/popup.html  339 bytes          [emitted]  
              popup/popup.js    116 KiB       3  [emitted]  popup/popup
```


## 참고:
- Content Security Policy (CSP): https://vuejsdevelopers.com/2017/05/08/vue-js-chrome-extension/
