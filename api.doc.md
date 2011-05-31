# -*- mode: org -*-

pyqqweibo document
==================

# API 列表

## timeline 时间线
* home 主页时间线
  翻页: pageflag+pagetime
      api.timeline.home()
      > [Tweet]
* public 广播大厅时间线
      api.timeline.public()
      > [Tweet]
* user 其他用户发表时间线
      api.timeline.user('andelf')
      > [Tweet]
* mentions 用户提及时间线
      api.timeline.mentions()
      > [Tweet]
* topic 话题时间线
      api.timeline.topic('CCTV')
      > [Tweet]
* broadcast 我发表时间线
  翻页: pageflag+pagetime
      api.timeline.broadcast()
      > [Tweet]
* special 特别收听的人发表时间线
      api.timeline.special()
      > [Tweet]
* area 地区发表时间线
      api.timeline.area(country=1, province=44, city=3)
      > [Tweet]
* homeids 主页时间线索引
      api.timeline.homeids()
      > [RetId] # RetId 可通过 ret.id, ret.timestamp 获取属性
* userids 其他用户发表时间线索引
      api.timeline.userids(name='NBA')
      [RetId]
* broadcastids 我发表时间线索引
      apt.timeline.broadcastids()
      > [RetId]
* mentionsids 用户提及时间线索引
      api.timeline.mentionsids()
      > [RetId]
* users 多用户发表时间线
      api.timeline.users(['name1,'name2','andelf'])
      > [Tweet]
* usersids 多用户发表时间线索引
      api.timeline.usersids(['name1,'name2','andelf'])
      > [Tweet]

* * *

## tweet 微博相关(t)
* show 获取一条微博数据
* add 发表一条微博
* delete 删除一条微博
* retweet 转播一条微博
* reply 回复一条微博
* addpic 发表一条带图片的微博
* retweetcount 转播数或点评数
      api.tweet.retweetcount(ids=[253446341312,34243234242]).as_dict()
      > {'34243234242': 0, ...}
* retweetlist 获取单条微博的转发或点评列表
* comment 点评一条微博
* addmusic 发表音乐微博
* addvideo 发表视频微博
* getvideoinfo 获取视频信息
* list 根据微博ID批量获取微博内容（与索引合起来用）
## user 帐户相关
* info 获取自己的详细资料
* update 更新用户信息
* updatehead 更新用户头像信息
* userinfo 获取其他人资料
## friends 关系链相关
* fanslist 我的听众列表
* idollist 我收听的人列表
* blacklist 黑名单列表
* speciallist 特别收听列表
* add 收听某个用户
* delete 取消收听某个用户
* addspecial 特别收听某个用户
* deletespecial 取消特别收听某个用户
* addblacklist 添加某个用户到黑名单
* deleteblacklist 从黑名单中删除某个用户
* check  检测是否我的听众或收听的人
    api.friends.check('andelf').as_dict()
    > {'andelf': False}
* userfanslist 其他帐户听众列表
    api.friends.userfanslist(name='andelf')
* useridollist 其他帐户收听的人列表
* userspeciallist 其他帐户特别收听的人列表
## private 私信相关
* add 发私信
* delete 删除一条私信
* inbox 收件箱
* outbox 发件箱
## search 搜索相关
* user 搜索用户
* tweet 搜索微博
* userbytag 通过标签搜索用户
## trends 热度，趋势
* topic 话题热榜
* tweet 转播热榜
    api.trends.tweet()
    [Tweet]
## info 数据更新相关
* update 查看数据更新条数
    api.info.update().as_dict()
    > {u'home': 21, u'create': 12, ...}
## fav 数据收藏
* addtweet 收藏一条微博
* deletetweet 从收藏删除一条微博
* listtweet 收藏的微博列表
* addtopic 订阅话题
* deletetopic 删除话题从收藏
* listtopic 获取已订阅话题列表
## topic 话题相关
* ids 根据话题名称查询话题ID
    api.topic.ids(u"地震")[0].id
* info 根据话题ID获取话题相关微博
    t = api.topic.info(5149259073282301489)[0]
    print t.text, t.tweetnum
## tag 标签相关
* add 添加标签
* delete 删除标签
## other 其他
* kownperson 我可能认识的人
    api.other.kownperson()
    > [User]
* shorturl 短URL变长URL
    # like http://url.cn/0jkApX
    api.other.shorturl('0jkApX').as_dict()
    > {'ctime': 0, 'longurl': u'http://...', 'secu': 3}
* videokey 获取视频上传的KEY
    api.other.videokey().as_dict()
    > {'uid': u'VNcmwzbqxdu=', 'videokey': u'$xMcNnpvswmmftd5pPkm'}

# Model 列表
## Tweet
* delete()
* retweet(content, clientip, jing=None, wei=None)
* reply(content, clientip, jing=None, wei=None)
* comment(content, clientip, jing=None, wei=None)
* retweetlist(#kwarg)
* retweetcount(flag=0)
* favorite()
* unfavorite()
## User
* self
    是否为自己
* update(##kwargs)
* timeline(##kwargs)
* add() / follow()
* delete() / unfollow()
* addspecial()
* deletespecial()
* addblacklist() / block()
* deleteblacklist() / unblock()
* fanslist(##kwargs) / followers()
* idollist(##kwargs) / followers()
* speciallist(##kwargs)
* pm(content, clientip, jing=None, wei=None)

# 翻页教程
## pageflag + pagetime
    api.timeline.home(reqnum=1)
    > [<Tweet object #76501075355511>]

    api.timeline.home(reqnum=1, pageflag=1, pagetime=_[-1].timestamp)
    > [<Tweet object #29107120390232>]

    api.timeline.home(reqnum=1, pageflag=1, pagetime=_[-1].timestamp)
    > [<Tweet object #78001074250068>]

## pos
不推荐使用使用 pos 翻页.

    pos = 0
    reqnum = 20
    ret = api.timeline.public(reqnum=reqnum, pos=pos)
    if len(ret)< reqnum:
        break
    pos += len(ret)
    ret = api.timeline.public(reqnum=reqnum, pos=pos)

## lastid
至今未成功过, 可见腾讯之垃圾.

## pageflag + pageinfo
TODO

