---
title: "React JS 기반 프로젝트 기동시 에러 유형 정리"
excerpt: "React JS 프로젝트 개발"

categories:
  - Script
tags:
  - Script 
classes: wide
last_modified_at: 2021-10-22T17:31:00-21:55:00
---

> 모든 것을 긍정적으로 보고, 차분히 미래를 준비한다. 

***

### npm install 시 python 2.7을 찾지 못하는 경우

https://github.com/felixrieseberg/windows-build-tools/issues/56    

npm install --global --production windows-build-tools   

### npm install 시 WINDOWS SDK 8.1이 없다고 할 경우 

 (Desktop_PlatformPrepareForBuild 대상) ->
  C:\Program Files (x86)\MSBuild\Microsoft.Cpp\v4.0\V140\Platforms\x64\PlatformToolsets\v140\Toolset.targets(36,5): err
or MSB8036: The Windows SDK version 8.1 was not found. Install the required version of Windows SDK or change the SDK ve
rsion in the project property pages or by right-clicking the solution and selecting "Retarget solution". [D:\SVN\00_Lin
es\S001_community\sample_proejects\react-idea\node_modules\node-sass\build\src\libsass.vcxproj]   

https://developer.microsoft.com/ko-kr/windows/downloads/sdk-archive/   
https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=websearch&logNo=221215760352   
 

### node-gyp 관련 이슈 발생시 

https://www.inflearn.com/questions/11540

npm install --global node-gyp


### node-sass 버전이 맞지 않을 경우, 

https://www.npmjs.com/package/node-sass