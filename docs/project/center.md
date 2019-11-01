## 软件需求

一个基于**开放社区管理**的社区和信息管理综合平台。

整个系统的所有功能包含在**社区**中，并使用**社区组**聚合类提供聚合功能。

社区的基础功能为发送**文章**，文章将包含必要的元数据，内容和附加类，以提供扩展的功能。文章的元数据：

扩展的功能主要功能为**活动**、**日程及事项**、**投票**。当然，也可以兼容**课程及课程更改**(包含于**日程及事项**)。

社区提供RRS源和扩展RRS源的订阅服务。

## 细节补充

### 用户系统（中心系统）

用户系统提供一个统一的登录和应用中心服务平台

用户系统（中心系统）将提供以下几个功能：

1. 基础的用户登录、注册相关的功能。

2. 应用管理和access_token的分发。

3. 由应用注册服务并声明所需要的权限。

4. 由应用声明使用的权限

5. 提供第三方应用登录的oauth2.0接口

下面是目前规划的数据库设计

#### 数据库设计





用户系统将提供基础的登录功能，设备验证功能和第三方登录功能。

同时，我们加入**第三方应用**,**服务提供者**和**服务权限分布**两个概念。主要用于扩展第三方服务的功能。

服务提供者指某一个应用注册了一个服务，

服务权限分布只要指一个应用使用的服务，以及用户对应用权限的授权情况，应用权限授权只有接受和拒绝两个选项。

用户的登录分为两种，直接token登录，oauth2.0

### 社区和社区组

**社区**是整个系统的核心组成部分，为了便于管理社区，加入了**社区组**的功能，但是社区组和社区是相对松散的。

### 文章

文章分为三个部分：元数据，内容和附加类。

#### 元数据

标题: 文章显示的标题，可以为空。

副标题：文章的副标题，可以为空。

署名：即文章所属版权，此选项为可选项，默认为创建此文章的人。

所属社区：标识文章属于哪一个社区，可以为空，当社区为空时，表示这是由用于创建的博客，并不属于任何社区。

创建者：文章的创建者。

参与者：文章的参与者，包含自动识别。

修改权限级别：文章的修改权限级别主要为private、protected。private指只有创建者可以修改。protected除创建者外，可以由其他成员参与编写工作。扩展选项由合作者来指定。

合作者：指可以参与文章编写的人员。

查看级别：指文章能够能够查看的人员，一般有private,protected,internal,public两个选项。private指只有自己可以查看，protected指只有社区内的成员可以查看，internal指只有关注该社区的成员可以查看。public指所有人员都可以查看。

标识：文章的标识，所有历史版本的文章标识都是唯一的。

历史版本：文章的历史版本

时间：指当前版本的时间

标签：指文章的标签，文章的标签分为全局和内部两种，具体信息请查看标签系统。并可以根据标签的不同类别提供不一样的功能。

文章媒体标识符：主要标识文章使用那一代媒体技术呈现内容。主要为*markdown*,*text*,*html*三种。

#### 文章内容

就是文章内容。当然，文章内容也可以是空的，此是，此文章必须使用附加数据选项。

#### 附加数据选项

主要标识文章的附加数据，一般包含附件、图片、附加活动、投票、日程及事项。此项使用引用来标识具体的附加数据。


### 扩展系统

扩展系统扩展了系统的功能，默认的有**活动**、**投票**和**日程和事项**。目前，先只介绍**日程和事项**扩展。

#### 日程和事项

单个日程和事项，并提供与社区有关的附加选项。

### 推荐系统

此系统也称为筛选系统，主要是从整个系统中筛选出用户需要的内容。

### 媒体管理

用于图片和媒体的管理

## API规范

所有自定义api的返回都是统一的，都是code,msg,data三个数据，code有正整数构成，表示该api返回结果的确定语义，一般将code分为三个段，用于表示三种类型的返回。

1. 0~99 表示返回的结果是正确的，是积极的语义。
2. 100~199 表示返回的结果是正确的，是消极的语义。
2. 200~299 表示与权限认证有关的错误
3. 300~399 表示与输入有关的错误
4. 400~499 表示其他类别的信息

其中code%100后若是0~49的表示此code是大部分api表示的同一个语义，例如请求成功等。50~99表示自定义语义。

200~499的消息表示软件本身发生了错误，需要软件直接过滤并提供友好提示。

msg提示明确指明操作的意义。例如创建账户成功，而不应该是请求成功等。

## 数据库设计