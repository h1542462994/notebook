
## Python后端实现

### 用户系统实体模型设计及说明

!!! tip "暂未使用的值"
    数据库模型中高亮的行表示因为版本原因暂时没有使用的值。

#### 用户信息

!!! summary "version 1.0.1"

    该表用于记录用户的基础信息，自1.0.1版本后，用户活动记录迁移到另外一张表{==UserActivity==}。

    ```python hl_lines="16 17 18"
    user_type_choices = (
        (0, 'normal'),
        (1, 'admin'),
        (2, 'superadmin')
    )

    @model('User')
    @search
    name: CharField(max_length=64, unique=True)  # 用户名，只能包含数字，大小写英文，下划线。（长度限制为2~32个字符）
    uid: CharField(max_length=64)  # 唯一标识名，由系统自己生成。（uid生成方式：时间戳+hash+10-99的随机数） 
    password: CharField(max_length=128)  # 密码，md5
    nickname: CharField(max_length=64)  # 昵称，1-32个任意字符
    email: CharField(default='',max_length=64)  # 邮箱
    create_time: DateTimeField()  # 创建时间
    user_type: PositiveSmallIntegerField(default=0, choices=user_type_choices)  # 用户类别，包括'normal','admin','superadmin'三种值，此信息仅用于管理用户信息，具体授权的操作见配置文件。
    exp: PositiveIntegerField(default=0)  # 用户经验
    tags: TextField(default='')  # 用户认证标签
    extras: TextField(default='')  # 无关紧要的额外的信息，以键值对形式存储，键为str，值为jsonobject/str
    @virtual
    activities: MultiForeignKey(UserActivity, name='activities')
    @virtual
    bindings: MultiForeignKey(UserBinding, name='bindings')
    @virtual
    logs: MultiForeignKey(UserLog, name='logs')
    ```

    !!! info "uid生成算法"
        取时间戳+用户名的hash值+10-99随机取一个随机数
        ```python
        def get_hash(value: str):
            md5 = hashlib.md5()
            md5.update(value.encode('utf-8'))
            return md5.hexdigest()

        def make_uid(name: str, time: float):
            a = str(int(time))
            b = get_hash(name)
            c = str(random.randint(10, 99))
            return a + b + c
        ```


??? summary "version 1.0.0"

    该表用于记录用户的基础信息

    ```python
    @model('User')
    @search
    name: CharField(max_length=64)  # 用户名，只能包含数字，大小写英文，下划线。（长度限制为2~32个字符）
    uid: CharField(max_length=64)  # 唯一标识名，由系统自己生成。（uid生成方式：时间戳+hash+10-99的随机数）
    password: CharField(max_length=128)  # 密码，取AES加密传输(md5 32位加密)
    nickname: CharField(max_length=64)  # 昵称，1-32个任意字符
    tokens: TextField()  # token，用于存储临时的登录信息
    email: CharField(max_length=64)  # 邮箱
    create_time: DateTimeField()  # 创建时间
    last_time: DateTimeField()  # 最后登录的时间
    ```

    !!! fail "token"
        以键值对的形式存储各个软件的目前登录信息，注册的软件写在另外一个数据表[config]内。

        token的生成方式为取字符串`uid:password(md5):last_time`进行AES加密，具体过程见附录。

        token的存储形式为{==softwarename::devicetype::token::last_ip::last_time==}例如：

        ```
        community::mobile::1231adef788231231234dddd::192.142.31.44:222::13213123123
        community::pc:123131231312312312312312::192.142.31.44:227::1231413423
        ```

#### 用户活动

!!! summary "version 1.0.1"

    该表用于记录用户的活动信息

    ```python hl_lines="13"
    device_type_choices = (
        (0, 'mobile'),
        (1, 'pc'),
        (2, 'web'),
        (3, 'pad'),
    )

    @model('UserActivity')
    @search
    user: ForeignKey(User, related_name='user_activities', on_delete=models.CASCADE)  # 外键，对应持有该活动信息用户。
    software: CharField(max_length=32)  # 对应的软件名称，默认支持'test'和'index'，其他支持需要询问平台配置。
    access_ip: CharField(max_length=32)  # 最后活动的ip地址
    # access_uid: CharField(max_length=64)  # -无效- 第一次登录时生成的uid，用于自动登录时的检测所用。
    device_type: PositiveSmallIntegerField(default=0, choices=device_type_choices)  # 设备平台名称，默认支持'mobile','pc','web','pad'四个平台的设备。
    access_time: DateTimeField()  # 最后活动的时间
    ```

    !!! info "token"
        该表并没有存储token信息，实际上，发给用户的信息将会转译成token

        token生成方式：生成字符串，格式为`useruid::software::device_type::access_uid::access_time`，例如，然后使用AES方式加密该字符串。

        除了'test'和'index'外，其他支持的软件需要进行后台注册。

        ```
        123143123123ac123::community::mobile::12313123123113acc::1231231231
        123143123123ac123::community::pc::12313123123113acc::1231231231
        ```

#### 用户绑定信息

!!! summary "version 1.0.1"

    该表用于记录用户的绑定信息，用于提供服务和使用第三方信息登录该软件。

    !!! tip "唯一绑定"
        每个第三方用户绑定是唯一的，即只能绑定在一个用户上，若要解绑，需要验证其他信息。

    ```python
    state_choices = (
        (0, 'success'),
        (1, 'fail'),
    )

    @model('UserBinding')
    @search
    user: ForeignKey(User, related_name='bindings', on_delete=models.CASCADE)  # 外键，对应持有该绑定信息的用户
    name: CharField(max_length=128)  # 绑定平台的名称，绑定的平台需要访问配置信息。
    uid: CharField(max_length=128)  # 唯一标识符，注意，此字段是唯一绑定的关键信息
    password: CharField(max_length=128)  # 绑定的密码
    state: PositiveSmallIntegerField(default=0, choices=state_choices)  # 绑定状态，'success','fail'.
    ```

#### 用户日志

!!! summary "version 1.0.1"

    用户的日志信息，每次登录/自动登录或者请求都可能往日志里写一条记录。

    ```python
    @model('UserLog')
    @search
    user: ForeignKey(User, related_name='user_log')  # 外键，对应持有该日志的用户
    action_type: CharField(max_length=64)  # 用户日志类别，所有兼容的类别都会在设置内写出
    time: DateTimeField()  # 打log的时间
    content: TextField()  # 用户日志内容
    ```

    预设的日志信息类别、格式和说明
    
    | action_type | format | description |
    | :---: | :---: | :--- :
    | login | state=enumof(success/fail)::software=str::access_ip=str::access_time=str | 用户登录活动 |

### 后端个性化配置

!!! summary ""

    ```python

    # 模式
    mode = 'debug'

    # 安全
    default_key = '****************'  # AES加密的默认密钥

    # 其他配置
    software = (
        'community'  # 社交软件
        'myday'  # 日程软件
    )

    ```

### API

!!! tip ""
    该部分还没有完成，请耐心等待

!!! tip "编写格式"
    请求按照{==yml==}的格式书写，格式为
    
    ```yml
    route: $route
    get:
        $argument1: $description
        ...
    post:
        $argument1: $decription
        ...
    ```

    返回的json格式如下
    ```json
    {
        "msg": "text",
        "shortcut": "text",
        "data": "jsonobject"
    }
    ```

    然后给出返回的shortcut值和对应的含义，并给出正确的输入和输出样例

#### 路径

```
本地开发环境: http://127.0.0.1:8080/
测试环境: 暂未部署
生产环境：暂未部署
```

#### 注册

注册分为很多种，有默认的注册，还有平台的注册。

##### 默认注册

!!! info ""
    **请求**

    ```python
    route: '/api/user/create'
    post(get in 'debug'):
        name: str  # 用户名，只能包含数字，大小写英文，下划线。（长度限制为2~32个字符）
        nickname: str  # 昵称，1-32个任意字符
        password: str  # 密码，md5，以16进制传输，英文字符为小写
        email: str  # 邮箱
    ```

    ---

    !!! info "查看用户是否已经被注册"
        我们对此提供了一个接口`check_name`用于检查用户名是否已经被注册

    **返回值**

    | shortcut | msg | 备注 |
    | :---: | :---: | :--- |
    | ae | arguments mismatch. | status=400,参数不匹配 |
    | fe | arguments format error:{0} | status=400,后面会给出详细错误信息，例如:arg(name) is invalid |
    | ur | the user [{name}] has been registerd. | 给出错误信息，表示该用户已经被注册 |
    | ok | ok, you have created a user, let's fun | 注册成功 |

    ---

    **样例**

    ```bat hl_lines="2 4"
    # get
    http://127.0.0.1:8000/api/user/create?name=test&nickname=test1&password=e10adc3949ba59abbe56e057f20f883e&email=example1@gmail.com
    # post
    http://127.0.0.1:8000/api/user/create
    name=test
    nickname=test1
    password=e10adc3949ba59abbe56e057f20f883e
    email=example1@gmail.com
    # return
    {
        "msg": "ok, you have create a user, let's fun", 
        "shortcut": "ok", 
        "data": {
            "name": "test", 
            "uid": "15631623408ad8757baa8564dc136c1e07507f4a9835", 
            "nickname": "test1", 
            "email": "example1@gmail.com", 
            "create_time": "2019-07-15 11:45:40", 
            "user_type": "normal", 
            "exp": 0, 
            "tags": "", 
            "extras": ""
        }
    }
    ```

#### 检查用户名是否已被注册

!!! info ""
    **请求**

    ```python
    route: '/api/user/check_name'
    get:
        name: str  # 要查询的用户名
    ```

    ---
     
    **返回值**

    | shortcut | msg | 备注 |
    | :---: | :---: | :--- |
    | ae | arguments mismatch | status=400,参数不匹配 |
    | fe | the format of name is invalid. | status=400,输出的姓名格式不正确 |
    | ur | the user [{user}] has been registered. | 该用户已被注册 |
    | ok | the user [{user}] has not been registerd. | 该用户没有被注册 |

    ---

    **样例**

    ```bat
    # get
    http://127.0.0.1:8000/api/user/check_name?name=test
    # return
    {
        "msg": "the user [test] has not been registered.",
        "shortcut": "ok"
    }
    ```

#### 登录

!!! info ""

    **请求**

    ```python
    route: '/api/user/login'
    post(get in 'debug'):
        name:str  # 登录的用户名，只能包含数字，大小写英文，下划线。（长度限制为2~32个字符）
        password:str  # 密码，md5，以16进制传输，英文字符为小写
        software:str  # 登录软件的名称，支持的软件#个性化设置栏目#
        device_type:str  # 登录的平台，支持'mobile','pc','web','pad'
    ```

    ---

    **返回值**

    | shortcut | msg | 备注 |
    | :---: | :---: | :--- |
    | ae | arguments mismatch. | status=400,参数不匹配 |
    | fe | arguments format error:{0} | status=400,后面会给出详细错误信息，例如:arg(name) is invalid |
    | une | user not existed. | 该用户不存在 |
    | upe | password error. | 用户密码错误 |
    | ok | login ok. let\'s fun. | 注册成功 |

    ---

    **样例**

    ```bat
    # get
    http://127.0.0.1:8000/api/user/login?name=test&password=e10adc3949ba59abbe56e057f20f883e&software=index&device_type=mobile
    # post
    http://127.0.0.1:8000/api/user/login
    name=test
    password=e10adc3949ba59abbe56e057f20f883e
    software=index
    device_type=mobile
    # result
    {
        "msg": "login ok. let's fun.", 
        "shortcut": "ok", 
        "data": {
            "name": "test", 
            "uid": "1563161937098f6bcd4621d373cade4e832627b4f623", 
            "nickname": "test1", 
            "email": "example1@gmail.com", 
            "create_time": "2019-07-15 11:38:57", 
            "user_type": "normal", 
            "exp": 0, 
            "tags": "", 
            "extras": "", 
            "access_time": "2019-07-15 16:57:18",
            "token": "9e14c58047a47370406a351bc607bbf4c0a4722e0688a5207192f80141c87513278b00f7c1a7ee8462fbc911d9c0109f48765b54107f87a857c1fd65c4ebc6b0e3598ac303d7c824c4724f7bb799d8a321584247fcaed41b539d0eb6268e31ec"
        }
    }
    ```

## Java后端实现

### 用户系统实体模型设计及说明

#### 用户信息

!!! summary "version 1.0.1"

    ```python
    `id`: IntegerField()  # 用户的唯一标识符
    password: TextField()  # 密码
    token: TextField()  # 用户的token信息
    last_login_ip: TextField()  # 用户最后一次登录的ip地址
    last_login_time: TextField()  # 用户最后一次登录的时间，格式为'%y-%M-%d'
    email: TextField()  # 用户邮箱
    log: TextField()  # 用户登录的日志
    nickname: TextField()  # 用户的昵称
    ```


## 其他辅助信息

### community用户系统的结构摘要[^2]

!!! summary ""

    ```java
    /*
    用户系统:
    {
        登录:
        [
            账号密码登录,
            根据token自动登录,
            第三方授权登录
        ]
        注册:
        [
            第三方授权登录补全信息,
            普通用户信息注册
        ]
        用户组:
        [
            超级管理员,
            普通管理员,
            版块版主,
            普通用户,
            封禁用户,
            游客
        ]
        个人信息:
        [
            用户id,
            账户密码,
            token信息,
            平台绑定信息,
            关注版块,
            学校绑定:
            [
                学校代码,
                学校城市代码,
                学校名称:
                [
                    中文,
                    英文,
                    简称
                ]
                专业,
                班级,
                学号,
                姓名,
                社团/组织
            ],
            昵称,
            简介,
            用户认证:
            [
                个人认证,
                企业认证,
                普通用户
            ],
            个性签名,
            姓名,
            邮箱,
            城市,
            地址,
            电话,
            爱好,
            关注,
            粉丝,
            推文数量,
            状态:[
                正常,
                注销,
                不活跃,
                封禁X天
            ]
            注册日期,
            最后一次登录IP,
            最后一次登录日期,
            备注信息
        ]
    }
    推文系统:
    [
        主要组成部分:
        [
            作者,
            发布时间,
            (标题),
            正文,
            图片,
            转发链,
            回复,
            点赞,
        ],
        附加部分:
        [
            编辑历史,
            状态,
            可见性:
            [
                仅自己可见,
                部分人可见,
                部分人不可见
            ],
            是否可评论,
            是否已删除,
            点击量,
            浏览量,
            被举报信息,
            附加信息
        ]
    ]
    */
    ```

[^2]: 详情见[Github页面](https://github.com/TropicalTeamYard/TTY-Community-Service/blob/master/src/servlet/Api.java)，本页面可能更新并不及时。