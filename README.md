从YouTube上爬取视频
========================

# 一. YouTube官方爬取

 ## (一) API Key 申请

- 按照教程[Part 1: Using YouTube’s Python API for Data Science](https://towardsdatascience.com/tutorial-using-youtubes-annoying-data-api-in-python-part-1-9618beb0e7ea)申请YouTube API Key

- 按照教程[Where to download your_client_secret_File.json file](https://stackoverflow.com/questions/52200589/where-to-download-your-client-secret-file-json-file)下载your_client_secret_File.json file

## (二) YouTube视频信息爬取

YouTube提供了多种视频爬取方式，以其中两种为例：一种是通过search query；一种是通过videoId。

### 1. search query

youtube_videos.py函数定义如下：

```python
# -*- coding: utf-8 -*-

# Sample Python code for youtube.channels.list
# See instructions for running these code samples locally:
# https://developers.google.com/explorer-help/guides/code_samples#python

import os
import google_auth_oauthlib.flow
import googleapiclient.discovery
import googleapiclient.errors

scopes = ["https://www.googleapis.com/auth/youtube.readonly"]

def main():
    # Disable OAuthlib's HTTPS verification when running locally.
    # *DO NOT* leave this option enabled in production.
    os.environ["OAUTHLIB_INSECURE_TRANSPORT"] = "1"

    api_service_name = "youtube"
    api_version = "v3"
    client_secrets_file = "client_secret.json"

    # Get credentials and create an API client
    flow = google_auth_oauthlib.flow.InstalledAppFlow.from_client_secrets_file(
        client_secrets_file, scopes)
    credentials = flow.run_console()
    youtube = googleapiclient.discovery.build(
        api_service_name, api_version, credentials=credentials)

    ## please see the link:   https://developers.google.com/youtube/v3/docs    to get more ways to use this API
    ## search query 
    # API only allow maxResults in range [0, 50]
    request = youtube.search().list(
        q='cat',
        type="video",
        part="id,snippet",
        maxResults=50
    )
    response = request.execute()
    print(response)


if __name__ == "__main__":
    main()
```

当运行成功时，response返回结果格式如下：

```json
{
  "kind": "youtube#searchListResponse",
  "etag": etag,
  "nextPageToken": string,
  "prevPageToken": string,
  "regionCode": string,
  "pageInfo": {
    "totalResults": integer,
    "resultsPerPage": integer
  },
  "items": [
    search Resource
  ]
}
```

其中，search Resource的格式如下：

```json
{
  "kind": "youtube#searchResult",
  "etag": etag,
  "id": {
    "kind": string,
    "videoId": string,
    "channelId": string,
    "playlistId": string
  },
  "snippet": {
    "publishedAt": datetime,
    "channelId": string,
    "title": string,
    "description": string,
    "thumbnails": {
      (key): {
        "url": string,
        "width": unsigned integer,
        "height": unsigned integer
      }
    },
    "channelTitle": string,
    "liveBroadcastContent": string
  }
}
```

### 2. videoId

youtube_videos.py函数定义如下：

```python
# -*- coding: utf-8 -*-

# Sample Python code for youtube.channels.list
# See instructions for running these code samples locally:
# https://developers.google.com/explorer-help/guides/code_samples#python

import os
import google_auth_oauthlib.flow
import googleapiclient.discovery
import googleapiclient.errors

scopes = ["https://www.googleapis.com/auth/youtube.readonly"]

def main():
    # Disable OAuthlib's HTTPS verification when running locally.
    # *DO NOT* leave this option enabled in production.
    os.environ["OAUTHLIB_INSECURE_TRANSPORT"] = "1"

    api_service_name = "youtube"
    api_version = "v3"
    client_secrets_file = "client_secret.json"

    # Get credentials and create an API client
    flow = google_auth_oauthlib.flow.InstalledAppFlow.from_client_secrets_file(
        client_secrets_file, scopes)
    credentials = flow.run_console()
    youtube = googleapiclient.discovery.build(
        api_service_name, api_version, credentials=credentials)

    ## please see the link: https://developers.google.com/youtube/v3/docs to get more ways to use this API
    ## videoId
    # API only allow maxResults in range [0, 50]
    request = youtube.videos().list(
        part="snippet, contentDetails, recordingDetails, localizations, statistics",
        id="Ks-_Mh1QhMc,c0KYU2j0TM4,eIho2S0ZahI"
    )

    response = request.execute()
    print(response)

if __name__ == "__main__":
    main()
```

当运行成功时，response返回结果格式如下：

```json
{
  "kind": "youtube#videoListResponse",
  "etag": etag,
  "nextPageToken": string,
  "prevPageToken": string,
  "pageInfo": {
    "totalResults": integer,
    "resultsPerPage": integer
  },
  "items": [
    video Resource
  ]
}
```

其中，video Resource的格式如下：

```json
{
  "kind": "youtube#video",
  "etag": etag,
  "id": string,
  "snippet": {
    "publishedAt": datetime,
    "channelId": string,
    "title": string,
    "description": string,
    "thumbnails": {
      (key): {
        "url": string,
        "width": unsigned integer,
        "height": unsigned integer
      }
    },
    "channelTitle": string,
    "tags": [
      string
    ],
    "categoryId": string,
    "liveBroadcastContent": string,
    "defaultLanguage": string,
    "localized": {
      "title": string,
      "description": string
    },
    "defaultAudioLanguage": string
  },
  "contentDetails": {
    "duration": string,
    "dimension": string,
    "definition": string,
    "caption": string,
    "licensedContent": boolean,
    "regionRestriction": {
      "allowed": [
        string
      ],
      "blocked": [
        string
      ]
    },
    "contentRating": {
      "acbRating": string,
      "agcomRating": string,
      "anatelRating": string,
      "bbfcRating": string,
      "bfvcRating": string,
      "bmukkRating": string,
      "catvRating": string,
      "catvfrRating": string,
      "cbfcRating": string,
      "cccRating": string,
      "cceRating": string,
      "chfilmRating": string,
      "chvrsRating": string,
      "cicfRating": string,
      "cnaRating": string,
      "cncRating": string,
      "csaRating": string,
      "cscfRating": string,
      "czfilmRating": string,
      "djctqRating": string,
      "djctqRatingReasons": [,
        string
      ],
      "ecbmctRating": string,
      "eefilmRating": string,
      "egfilmRating": string,
      "eirinRating": string,
      "fcbmRating": string,
      "fcoRating": string,
      "fmocRating": string,
      "fpbRating": string,
      "fpbRatingReasons": [,
        string
      ],
      "fskRating": string,
      "grfilmRating": string,
      "icaaRating": string,
      "ifcoRating": string,
      "ilfilmRating": string,
      "incaaRating": string,
      "kfcbRating": string,
      "kijkwijzerRating": string,
      "kmrbRating": string,
      "lsfRating": string,
      "mccaaRating": string,
      "mccypRating": string,
      "mcstRating": string,
      "mdaRating": string,
      "medietilsynetRating": string,
      "mekuRating": string,
      "mibacRating": string,
      "mocRating": string,
      "moctwRating": string,
      "mpaaRating": string,
      "mpaatRating": string,
      "mtrcbRating": string,
      "nbcRating": string,
      "nbcplRating": string,
      "nfrcRating": string,
      "nfvcbRating": string,
      "nkclvRating": string,
      "oflcRating": string,
      "pefilmRating": string,
      "rcnofRating": string,
      "resorteviolenciaRating": string,
      "rtcRating": string,
      "rteRating": string,
      "russiaRating": string,
      "skfilmRating": string,
      "smaisRating": string,
      "smsaRating": string,
      "tvpgRating": string,
      "ytRating": string
    },
    "projection": string,
    "hasCustomThumbnail": boolean
  },
  "status": {
    "uploadStatus": string,
    "failureReason": string,
    "rejectionReason": string,
    "privacyStatus": string,
    "publishAt": datetime,
    "license": string,
    "embeddable": boolean,
    "publicStatsViewable": boolean,
    "madeForKids": boolean,
    "selfDeclaredMadeForKids": boolean
  },
  "statistics": {
    "viewCount": unsigned long,
    "likeCount": unsigned long,
    "dislikeCount": unsigned long,
    "favoriteCount": unsigned long,
    "commentCount": unsigned long
  },
  "player": {
    "embedHtml": string,
    "embedHeight": long,
    "embedWidth": long
  },
  "topicDetails": {
    "topicIds": [
      string
    ],
    "relevantTopicIds": [
      string
    ],
    "topicCategories": [
      string
    ]
  },
  "recordingDetails": {
    "recordingDate": datetime
  },
  "fileDetails": {
    "fileName": string,
    "fileSize": unsigned long,
    "fileType": string,
    "container": string,
    "videoStreams": [
      {
        "widthPixels": unsigned integer,
        "heightPixels": unsigned integer,
        "frameRateFps": double,
        "aspectRatio": double,
        "codec": string,
        "bitrateBps": unsigned long,
        "rotation": string,
        "vendor": string
      }
    ],
    "audioStreams": [
      {
        "channelCount": unsigned integer,
        "codec": string,
        "bitrateBps": unsigned long,
        "vendor": string
      }
    ],
    "durationMs": unsigned long,
    "bitrateBps": unsigned long,
    "creationTime": string
  },
  "processingDetails": {
    "processingStatus": string,
    "processingProgress": {
      "partsTotal": unsigned long,
      "partsProcessed": unsigned long,
      "timeLeftMs": unsigned long
    },
    "processingFailureReason": string,
    "fileDetailsAvailability": string,
    "processingIssuesAvailability": string,
    "tagSuggestionsAvailability": string,
    "editorSuggestionsAvailability": string,
    "thumbnailsAvailability": string
  },
  "suggestions": {
    "processingErrors": [
      string
    ],
    "processingWarnings": [
      string
    ],
    "processingHints": [
      string
    ],
    "tagSuggestions": [
      {
        "tag": string,
        "categoryRestricts": [
          string
        ]
      }
    ],
    "editorSuggestions": [
      string
    ]
  },
  "liveStreamingDetails": {
    "actualStartTime": datetime,
    "actualEndTime": datetime,
    "scheduledStartTime": datetime,
    "scheduledEndTime": datetime,
    "concurrentViewers": unsigned long,
    "activeLiveChatId": string
  },
  "localizations": {
    (key): {
      "title": string,
      "description": string
    }
  }
}
```

### 3. 提取信息

根据resource的json结果，可以提取其中的信息，从而进行下一步统计。

```python
for search_result in search_response.get("items", []):
    if search_result["id"]["kind"] == "youtube#video":
        videos.append(search_result)
```

# 二. selenium模拟浏览器下载youtube视频

> 由于官方youtube查询时最多只能返回50个查询结果，每次查询时返回结果几乎一样，另外，youttube需要鼠标滑动到页面底端才会有刷新，新一批的视频才会召回，而此时url并没有变化，静态爬虫无法满足。因此，为了能够爬取类似网页的信息，可以采用selenium来模拟鼠标操作浏览器，实现动态爬虫。

```python
#! /usr/bin/env python3
# author: Qi Shao

########### load packages ############
from selenium import webdriver
import time
from bs4 import BeautifulSoup


########### 打开Chrome浏览器 ############
# chromedriver下载地址： http://npm.taobao.org/mirrors/chromedriver/
driver = webdriver.Chrome(executable_path="/home/sensetime/Desktop/code/anet_dataset/chromedriver")
driver.get("https://www.youtube.com/")


########### 窗口最大化 ############
driver.maximize_window()
time.sleep(1)
driver.refresh()


########### 获取cookie ############
cookie = driver.get_cookies()


########### 查询query ############
for query in ['cat', 'dog']:

    ########### 查询query，限制video时长在4分钟以内 ############
    url = 'https://www.youtube.com/results?search_query=' + query + '&sp=EgQQARgB'
    driver.get(url)
    print(query)

    def execute_times(times):
        for i in range(times + 1):
            ########### 解析html ############
            html = driver.page_source
            soup = BeautifulSoup(html, 'lxml')
            zzr = soup.find_all('a', id="thumbnail")

            ########### 获取video_id ############
            for item in zzr:
                video = item.get("href")
                if video is not None and "/watch?v=" in video:
                    video_id = video.replace('/watch?v=', '')
                    print(video_id)

            ########### 模拟鼠标向下滑动 ############
            js = "var q=document.documentElement.scrollTop=100000000000"
            driver.execute_script(js)
            time.sleep(3)  # 等待页面刷新

    ########### 模拟鼠标向下滑动3次 ############
    execute_times(3)
    time.sleep(1)

########### 退出Chrome ############
driver.quit()
```

参考：https://www.zhihu.com/question/46528604

selenium参考：https://selenium-python.readthedocs.io/installation.html

# 三. YouTube视频下载

Youtube视频下载可以使用youtube-dl工具，具体参考：https://github.com/ytdl-org/youtube-dl ，使用方法可以参考： https://zhuanlan.zhihu.com/p/27718783 ，批量生成下载命令可以参考：https://github.com/activitynet/ActivityNet/blob/master/Crawler/run_crosscheck.py
