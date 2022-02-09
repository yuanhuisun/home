# Laravel学习系列

————————————————
原文作者：Laravel China &#31038;&#21306;&#25991;&#26723;&#65306;&#12298;Laravel 8 &#20013;&#25991;&#25991;&#26723;&#65288;8.5&#65289;&#12299;  
转自链接：https://learnku.com/docs/laravel/8.5/structure/10361  
&#29256;&#26435;&#22768;&#26126;&#65306;&#32763;&#35793;&#25991;&#26723;&#33879;&#20316;&#26435;&#24402;&#35793;&#32773;&#21644; LearnKu &#31038;&#21306;&#25152;&#26377;&#12290;&#36716;&#36733;&#35831;&#20445;&#30041;&#21407;&#25991;&#38142;&#25509;  



## 从这里开始
在开始学习之前，你需要安装好[Laravel](https://laravel.com/)和[Laravel Breeze](https://laravel.com/docs/8.x/starter-kits) as a starting point.
- 安装Laravel开发环境：Homestead或者Docker
- 安装Laravel Framework 8.X
- 安装Laravel Breeze

## Laravel Framework

### Laravel应用文件目录结构
默认的 Laravel 应用程序结构旨在为大型和小型应用程序提供一个良好的起点。但是你可以自由地组织你的应用程序。只要 Composer 可以自动加载类，Laravel 几乎不限制任何给点类的位置。

#### 根目录
Laravel根目录包括app、bootstrap、config、database、public、resources、routes、storage、tests和vendor等子目录。
- app : 包含应用程序的核心代码，我们将在稍后详细探讨该目录；不管怎样，应用程序中几乎所有的类都位于此目录中。
- bootstrap : 包含了框架的启动文件 app.php。该目录还包含一个 cache 目录，其中包含框架生成的文件，这些文件用于性能优化，例如路由和服务缓存文件。你通常不需要修改此目录中的任何文件。
- config : 包含应用程序的所有配置文件。最好把这些文件都浏览一遍，并熟悉所有可用的选项。
- database : 包含数据库迁移，模型工厂和种子生成器文件。如果你愿意，你还可以把它作为 SQLite 数据库存放目录。
- public : 包含 index.php 文件，该文件是进入你应用程序的所有请求的入口，并配置自动加载。该目录还包含你的资源，如图像、JavaScript 脚本和 CSS 样式。
- resources : 包含了 views 以及未编译的资源文件（如 CSS 或 JavaScript）。此目录还包含所有的语言文件。
- routes : 包含应用程序的所有路由定义。默认情况下，Laravel 包含几个路由文件：web.php，api.php，console.php 以及 channels.php。  
  web.php 文件包含 RouteServiceProvider 放置在 web 中间件组中的路由，该中间件组提供会话状态，CSRF 保护和 cookie 加密，如果你的应用程序不提供无状态的 RESTful API，那么你的所有路由都很可能在 web.php 文件。
  api.php 文件包含 RouteServiceProvider 放置在 api 中间件组中的路由。这些路由是无状态的，因此通过这些路由进入应用程序的请求将 通过令牌 进行身份验证，并且将无法访问会话状态。
  console.php 文件中定义所有基于闭包的控制台命令。每个闭包都被绑定到一个命令实例，从而允许一种简单的方法与每个命令的 IO 方法进行交互。即使此文件未定义 HTTP 路由，它仍定义了应用程序中基于控制台的入口点 (路由) 。
  channels.php 文件是你可以注册应用程序支持的所有 事件广播 频道的位置。
- storage : 包含了你的日志文件，已编译的 Blade 模版，基于文件的会话，文件缓存以及框架生成的其他文件。该目录分为 app，framework 和 logs 目录。app 目录可用于存储应用程序生成的任何文件。framework 目录用于存储框架生成的文件和缓存。最后，logs 目录包含应用程序的日志文件。  
  storage/app/public 目录用来存储用户生成的文件，例如个人资料，这些文件应该是可以公开访问的。你应该在 public/storage 处创建一个指向到该目录的链接。你可以使用 php artisan storage:link 命令来创建链接。
- tests : 包含自动化测试类。开箱即用的示例 PHPUnit 单元测试和功能测试。每个测试类都应该以单词 Test 作为后缀。你可以使用 phpunit 或 php vendor/bin/phpunit 命令运行测试。或者，如果你想更详细，更漂亮地表示测试结果，则可以使用 php artisan test 命令运行测试。

- vendor : 包含你的 Composer 依赖。

#### app目录

应用程序的大部分都位于 app 目录中。默认情况下，此目录的名称空间在 App 下，并由 Composer 使用 PSR-4 自动加载 自动加载。  

app 目录包含各种目录，如 Console、Http 和 Providers。可以将 Console 和 Http 目录看作是为应用程序的核心提供了一个 API。HTTP 协议和 CLI 都是与应用程序交互的机制，但实际上并不包含应用程序逻辑。换句话说，它们是向应用程序发出命令的两种方式。Console 目录包含所有 Artisan 命令，而 Http 目录包含控制器、中间件和请求。  

当你使用 Artisan 命令 make 生成类时，将在 app 目录内生成各种目录。因此，例如当你执行 make:job 命令生成队列任务时，将会生成 app/Jobs 目录。  

  技巧：app 目录中的许多类可以由 Artisan 通过命令生成。要查看可用的命令，请在终端中运行 php artisan list make 命令。  

  - Broadcasting 目录
  - Console 目录
  - Events 目录
  - Exceptions 目录
  - Http 目录
  - Jobs 目录
  - Listeners 目录
  - Mail 目录
  - Models 目录
  - Notifications 目录
  - Policies 目录
  - Providers 目录
  - Rules 目录


### 理解Laravel基础架构


## 用户认证与授权

## 基于Laravel Blade的前端设计(Front-end Design with Laravel Blade) 

## 数据库设计 Database Design

## 应用路由