## 闲话
前几天某音推出了网页版，相较于之前的分享页，新的网页版功能还是比较完善的，视频信息、用户信息、搜索都可以用，而且目前来看，只有一个加密参数和部分接口需要cookie。
如果仅仅想抓取指定用户的视频，可以移步我之前的文章: [网页版（分享页）某音signature获取](https://blog.csdn.net/wang785994599/article/details/115175876)，代码也已经开源。

新的网页版某音常用接口如下，后续还会丰富。
**本文接口基于JS逆向，直接node运行生成signature**
**需要token进行测试的，+v wang785994599**
## 近期更新
- 2021-07-13更新
	已经可以实现点赞和关注接口的加签（仅供交流学习，请勿用于商业或其他途径）
- 2021-07-21 完成dy网页版扫码登录
## 接下来要做的
- 手机端cookie转网页版cookie

## 用户相关接口

- 用户信息

```
https://www.douyin.com/aweme/v1/web/user/profile/other/?device_platform=webapp&aid=6383&channel=channel_pc_web&publish_video_strategy_type=2&source=channel_pc_web&sec_user_id=MS4wLjABAAAAgq8cb7cn9ByhZbmx-XQDdRTvFzmJeBBXOUO4QflP96M&version_code=160100&version_name=16.1.0
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/img_convert/97be6c2dc71f92f9fb3e4dd59b02103e.png)

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
![在这里插入图片描述](https://img-blog.csdnimg.cn/img_convert/5d85814bc696f6b5923152485ffe3bd2.png)

- 用户搜索

```
https://www.douyin.com/aweme/v1/web/discover/search/?device_platform=webapp&aid=6383&channel=channel_pc_web&search_channel=aweme_user_web&keyword=编码后的关键字&search_source=normal_search&query_correct_type=1&is_filter_search=0&offset=0&count=18&version_code=160100&version_name=16.1.0
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/img_convert/adf2d05e9f3a17a2f42a8a093ae84f4a.png)



编码样例

```
央视新闻 ==> %E5%A4%AE%E8%A7%86%E6%96%B0%E9%97%BB
```

## 视频相关

- 获取视频评论

```
https://www.douyin.com/aweme/v1/web/comment/list/?device_platform=webapp&aid=6383&channel=channel_pc_web&aweme_id=6979760336979217694&cursor=20&count=20&version_code=160100&version_name=16.1.0
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/img_convert/118831eb78cf51c007a228a221118300.png)
## 行为接口
请勿用于非法用途！！！
- 点赞
```
https://www.douyin.com/aweme/v1/web/commit/item/digg/?device_platform=webapp&aid=6383&channel=channel_pc_web&version_code=160100&version_name=16.1.0&cookie_enabled=true&screen_width=1536&screen_height=864&browser_language=zh-CN&browser_platform=Win32&browser_name=Mozilla&browser_version=5.0+(Windows+NT+10.0%3B+Win64%3B+x64)+AppleWebKit%2F537.36+(KHTML,+like+Gecko)+Chrome%2F91.0.4472.101+Safari%2F537.36&browser_online=true
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/img_convert/1cb82c09367ac83e5fe4cf109da053be.png)
返回值中 is_digg=0即代表成功
- 关注
```
https://www.douyin.com/aweme/v1/web/commit/follow/user/?device_platform=webapp&aid=6383&channel=channel_pc_web&version_code=160100&version_name=16.1.0
 ```
![在这里插入图片描述](https://img-blog.csdnimg.cn/img_convert/0d9383f22ed8a485b81fedcc0d2c06e0.png)
返回值中存在is_enterprise字段即代表成功
## 待完成接口
 
 - ~~点赞\取消点赞~~
 - ~~关注\取消关注~~




## 测试接口以及测试代码[python]
目前提供接口供测试使用，需要**token**的可以私信我，每个token有效期为1小时，访问频率限制为每分钟30次，且测且珍惜，如果有什么疑问可私信，**上来就要代码的，勿扰**


```python3
# -*- coding:utf-8 -*-
"""
@desc: 
"""
import pprint
from urllib.parse import urlencode, quote

import requests

headers = {
    'user-agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/91.0.4472.101 Safari/537.36',
    'referer': 'https://www.douyin.com/video/6981060082599529764?previous_page=others_homepage',
}
session = requests.session()
session.headers = headers

token = 'your token'


def do_request(url, login=False):
    data = {
        "url": url
    }
    resp = requests.post('http://59.110.158.68:8787/sign', data=data, headers={
        "Authorization":token
    }).json()
    print(resp)
    if resp['error_code'] == 0:
        _signature = resp['result']['_signature']
        url += "&" + urlencode({"_signature": _signature})
        response = session.get(url)
        return response


def user_search(keyword):
    keyword = quote(keyword)
    url = f'https://www.douyin.com/aweme/v1/web/discover/search/?device_platform=webapp&aid=6383&channel=channel_pc_web&search_channel=aweme_user_web&keyword={keyword}&search_source=normal_search&query_correct_type=1&is_filter_search=0&offset=0&count=18&version_code=160100&version_name=16.1.0'
    response = do_request(url)
    if response:
        users = ['%s %s' % (i['user_info']['uid'], i['user_info']['nickname']) for i in response.json()['user_list']]
        print('\n'.join(users))


def item_search(keyword):
    keyword = quote(keyword)
    url = f'https://www.douyin.com/aweme/v1/web/search/item/?device_platform=webapp&aid=6383&channel=channel_pc_web&search_channel=aweme_video_web&sort_type=0&publish_time=0&keyword={keyword}&search_source=normal_search&query_correct_type=1&is_filter_search=0&offset=0&count=24&version_code=160100&version_name=16.1.0&cookie_enabled=true&screen_width=1536&screen_height=864&browser_language=zh-CN&browser_platform=Win32&browser_name=Mozilla&browser_version=5.0+(Windows+NT+10.0%3B+Win64%3B+x64)+AppleWebKit%2F537.36+(KHTML,+like+Gecko)+Chrome%2F91.0.4472.101+Safari%2F537.36&browser_online=true'
    response = do_request(url)
    if response:
        users = ['%s %s' % (i['aweme_info']['aweme_id'], i['aweme_info']['desc']) for i in response.json()['data']]
        print('\n'.join(users))


def user_info():
    session.get('https://www.douyin.com')
    url = 'https://www.douyin.com/aweme/v1/web/user/profile/other/?device_platform=webapp&aid=6383&channel=channel_pc_web&publish_video_strategy_type=2&source=channel_pc_web&sec_user_id=MS4wLjABAAAAAEtO1dCIZvj4VWbLU4Xce7DgVgsKNMNu88eNR2c2LtY&version_code=160100&version_name=16.1.0&cookie_enabled=true&screen_width=1920&screen_height=1080&browser_language=zh-CN&browser_platform=Win32&browser_name=Mozilla&browser_version=5.0+(Windows+NT+10.0%3B+Win64%3B+x64)+AppleWebKit%2F537.36+(KHTML,+like+Gecko)+Chrome%2F90.0.4430.93+Safari%2F537.36&browser_online=true'
    response = do_request(url)
    if response:
        print(response.json()['user'])


def user_post():
    url = 'https://www.douyin.com/aweme/v1/web/aweme/post/?device_platform=webapp&aid=6383&channel=channel_pc_web&sec_user_id=MS4wLjABAAAAgq8cb7cn9ByhZbmx-XQDdRTvFzmJeBBXOUO4QflP96M&max_cursor=1625099137000&count=10&publish_video_strategy_type=2&version_code=160100&version_name=16.1.0'

    response = do_request(url)
    if response:
        users = ['%s %s' % (i['aweme_id'], i['desc']) for i in response.json()['aweme_list']]
        print('\n'.join(users))


def user_favorite():
    session.get('https://www.douyin.com')

    url = 'https://www.douyin.com/aweme/v1/web/aweme/favorite/?device_platform=webapp&aid=6383&channel=channel_pc_web&sec_user_id=MS4wLjABAAAAgq8cb7cn9ByhZbmx-XQDdRTvFzmJeBBXOUO4QflP96M&max_cursor=0&min_cursor=0&count=10&publish_video_strategy_type=2&version_code=160100&version_name=16.1.0'
    response = do_request(url)
    if response:
        users = ['%s %s' % (i['aweme_id'], i['desc']) for i in response.json()['aweme_list']]
        print('\n'.join(users))


def item_comment(aweme_id=6985096535633628453):
    url = f'https://www.douyin.com/aweme/v1/web/comment/list/?device_platform=webapp&aid=6383&channel=channel_pc_web&aweme_id={aweme_id}&cursor=20&count=20&version_code=160100&version_name=16.1.0'

    session.get('https://www.douyin.com')
    response = do_request(url)
    if response:
        users = ['%s %s' % (i['user']['nickname'], i['text']) for i in response.json()['comments']]
        print('\n'.join(users))


if __name__ == '__main__':
    # user_search('陈赫')
    # item_search('迈巴赫')
    item_comment()
    # user_favorite()
    # user_post()
    # user_info()

```



免责声明：此代码仅学习使用，请勿用于商业用途，如拿去非法使用与本人无关！
