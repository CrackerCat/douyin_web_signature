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

## 点赞和关注接口的测试脚本
```
# -*- coding:utf-8 -*-
"""
@desc: 
"""
import requests

# from scripts import x_secsdk_csrf_token, cookie

cookie = '' # 你的cookie
x_secsdk_csrf_token = '' # 最新的一次点赞或关注，请求头中的x_secsdk_csrf_token
headers = {
    'authority': 'www.douyin.com',
    'sec-ch-ua-mobile': '?0',
    'user-agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.101 Safari/537.36',
    'accept': '*/*',
    'sec-fetch-site': 'same-origin',
    'sec-fetch-mode': 'cors',
    'x-secsdk-csrf-token': x_secsdk_csrf_token,
    'sec-fetch-dest': 'empty',
    'referer': 'https://www.douyin.com/user/MS4wLjABAAAAgq8cb7cn9ByhZbmx-XQDdRTvFzmJeBBXOUO4QflP96M?enter_method=video_title&author_id=66598046050&group_id=6976510986547105060&log_pb=%7B%22impr_id%22%3A%22021624413357501fdbddc0100fff0020a9b342d0000001b0ac6ec%22%7D&enter_from=video_detail',
    # 'accept-language': 'zh-CN,zh;q=0.9',
    'cookie': cookie
}
server_url = 'http://59.110.158.68:8787/sign_v3'


def digg(aweme_id="6984008368784477454"):
    digg_url = 'https://www.douyin.com/aweme/v1/web/commit/item/digg/?device_platform=webapp&aid=6383&channel=channel_pc_web&version_code=160100&version_name=16.1.0&cookie_enabled=true&screen_width=1536&screen_height=864&browser_language=zh-CN&browser_platform=Win32&browser_name=Mozilla&browser_version=5.0+(Windows+NT+10.0%3B+Win64%3B+x64)+AppleWebKit%2F537.36+(KHTML,+like+Gecko)+Chrome%2F91.0.4472.101+Safari%2F537.36&browser_online=true'

    data = {
        "url": digg_url,
        "body": "type=1&aweme_id=%s" % aweme_id
    }
    response = requests.post(server_url, data=data).json()
    print(response)
    if response['error_code'] == 0:
        sign = response['result']['_signature']
        if sign:
            digg_url += "&_signature=" + sign
            response = requests.post(digg_url, headers=headers, data={
                "aweme_id": aweme_id,
                "type": "1"
            })
            print(response.json())
            assert response.json().get("is_digg") == 0  # 点赞成功标志


def follow(user_id="66598046050"):
    follow_url = "https://www.douyin.com/aweme/v1/web/commit/follow/user/?device_platform=webapp&aid=6383&channel=channel_pc_web&version_code=160100&version_name=16.1.0"
    data = {
        "url": follow_url,
        "body": "type=1&user_id=%s" % user_id
    }
    response = requests.post(server_url, data=data).json()
    print(response)
    if response['error_code'] == 0:
        sign = response['result']['_signature']
        if sign:
            follow_url += "&_signature=" + sign
            data = {
                "type": "1",
                "user_id": user_id
            }
            response = requests.post(follow_url, headers=headers, data=data)
            try:
                print(response.json())
            except:
                print(response.text)

if __name__ == '__main__':
    # digg()
    follow()
```

