## 网页版抖音（douyin.com）signature

## 闲话
前几天抖音推出了网页版，相较于之前的分享页，新的网页版功能还是比较完善的，视频信息、用户信息、搜索都可以用，而且目前来看，只有一个加密参数和部分接口需要cookie。
如果仅仅想抓取指定用户的视频，可以移步我之前的文章: [网页版（分享页）抖音signature获取](https://blog.csdn.net/wang785994599/article/details/115175876)，代码也已经开源。

新的网页版抖音(douyin.com)常用接口如下，后续还会丰富。
## 用户相关接口

- 用户信息

```
https://www.douyin.com/aweme/v1/web/user/profile/other/?device_platform=webapp&aid=6383&channel=channel_pc_web&publish_video_strategy_type=2&source=channel_pc_web&sec_user_id=MS4wLjABAAAAgq8cb7cn9ByhZbmx-XQDdRTvFzmJeBBXOUO4QflP96M&version_code=160100&version_name=16.1.0
```

- 用户视频列表

`````
https://www.douyin.com/aweme/v1/web/aweme/post/?device_platform=webapp&aid=6383&channel=channel_pc_web&sec_user_id=MS4wLjABAAAAgq8cb7cn9ByhZbmx-XQDdRTvFzmJeBBXOUO4QflP96M&max_cursor=1625099137000&count=10&publish_video_strategy_type=2&version_code=160100&version_name=16.1.0
`````

- 用户喜欢视频列表

```
https://www.douyin.com/aweme/v1/web/aweme/favorite/?device_platform=webapp&aid=6383&channel=channel_pc_web&sec_user_id=MS4wLjABAAAAgq8cb7cn9ByhZbmx-XQDdRTvFzmJeBBXOUO4QflP96M&max_cursor=0&min_cursor=0&count=10&publish_video_strategy_type=2&version_code=160100&version_name=16.1.0
```





## 搜索

- 视频搜索

```
https://www.douyin.com/aweme/v1/web/search/item/?device_platform=webapp&aid=6383&channel=channel_pc_web&search_channel=aweme_video_web&sort_type=0&publish_time=0&keyword=编码后的关键字&search_source=normal_search&query_correct_type=1&is_filter_search=0&offset=0&count=24&version_code=160100&version_name=16.1.0
```

- 用户搜索

```
https://www.douyin.com/aweme/v1/web/discover/search/?device_platform=webapp&aid=6383&channel=channel_pc_web&search_channel=aweme_user_web&keyword=编码后的关键字&search_source=normal_search&query_correct_type=1&is_filter_search=0&offset=0&count=18&version_code=160100&version_name=16.1.0
```



编码样例

```
央视新闻 ==> %E5%A4%AE%E8%A7%86%E6%96%B0%E9%97%BB
```

## 视频相关

- 获取视频评论

```
https://www.douyin.com/aweme/v1/web/comment/list/?device_platform=webapp&aid=6383&channel=channel_pc_web&aweme_id=6979760336979217694&cursor=20&count=20&version_code=160100&version_name=16.1.0
```

## 测试接口以及测试代码[python]
目前提供接口供测试使用，鉴于服务器配置低限制单个ip每日访问10次，且测且珍惜，如果有什么疑问可以加V或其他的需求 wang785994599

`http://59.110.158.68:8787/sign`

```python
#!usr/bin/env python
# -*- coding:utf-8 -*-
"""
@author: coderfly
@file: demo
@time: 2021/4/20
@email: coderflying@163.com
@desc:
"""
import time

import requests
from urllib.parse import urlencode, urljoin

headers = {
    'user-agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.101 Safari/537.36',
    'referer': 'https://www.douyin.com/user/MS4wLjABAAAAgq8cb7cn9ByhZbmx-XQDdRTvFzmJeBBXOUO4QflP96M?enter_method=video_title&author_id=66598046050&group_id=6976510986547105060&log_pb=%7B%22impr_id%22%3A%22021624413357501fdbddc0100fff0020a9b342d0000001b0ac6ec%22%7D&enter_from=video_detail',


}
session = requests.session()
session.headers = headers
# session.get('https://www.douyin.com') # 部分请求可能需要cookie，如获取用户喜欢列表，加一步获取cookie即可
url = "https://www.douyin.com/aweme/v1/web/aweme/post/?device_platform=webapp&aid=6383&channel=channel_pc_web&sec_user_id=MS4wLjABAAAAgq8cb7cn9ByhZbmx-XQDdRTvFzmJeBBXOUO4QflP96M&max_cursor=1623331736000&count=10&publish_video_strategy_type=2&version_code=160100&version_name=16.1.0"

data = {
    "method":"get",
    "url":url
}
resp = requests.post('http://59.110.158.68:8787/sign', data=data).json()
if resp['error_code'] == 0:
    _signature = resp['result']['_signature']
    url += "&" + urlencode({"_signature":_signature})
    response = session.get(url)
    print(response.json())


```

