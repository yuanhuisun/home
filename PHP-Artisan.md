## PHP artisan 命令列表
Laravel Framework 8.80.0

### Artisan工具简介  
Artisan 是 Laravel 中自带的命令行工具的名称。它提供了一些对您的应用开发有帮助的命令。它是由强大的 Symfony Console 组件驱动的。为了查看所有可用的 Artisan 的命令，您可以使用 list 命令来列出它们：

在脚本中执行php artisan list可以查看所有的命令  

那么熟悉linux的知道,不管什么命令都有一个help命令,当运行的时候,忽然之间,忘了的话,可以执行help命令去查看一下我们需要的命令,同样的在laravel框架中也可以去执行help命令去查看如: 
php artisan help migrate  

### 全局篇

#### 查看artisan命令
  php artisan
  php artisan list

#### 查看某个帮助命令 
  php artisan help make:model

#### 查看laravel版本
  php artisan --version

#### 使用 PHP 内置的开发服务器启动应用
  php artisan serve

#### 生成一个随机的 key，并自动更新到 app/config/app.php 的 key 键值对（刚安装好需要做这一步）
  php artisan key:generate

#### 开启Auth用户功能（开启后需要执行迁移才生效）
  php artisan make:auth

#### 开启维护模式和关闭维护模式（显示503）
  php artisan down
  php artisan up
#### 进入tinker工具
  php artisan tinker

#### 列出所有的路由
  php artisan route:list

#### 生成路由缓存以及移除缓存路由文件
  php artisan route:cache
  php artisan route:clear

### 功能篇

#### 创建控制器
  php artisan make:controller StudentController

#### 创建Rest风格资源控制器（带有index、create、store、edit、update、destroy、show方法 ）
  php artisan make:controllerPhotoController--resource

#### 创建模型
  php artisan make:model Student
#### 创建新建表的迁移和修改表的迁移
  php artisan make:migration create_users_table--create=students//创建students表
  php artisan make:migration add_votes_to_users_table--table=students//给students表增加votes字段

#### 执行迁移
  php artisan migrate

#### 创建模型的时候同时生成新建表的迁移
  php artisan make:modelStudent-m

#### 回滚上一次的迁移
  php artisan migrate:rollback
#### 回滚所有迁移
  php artisan migrate:reset

#### 创建填充
  php artisan make:seeder StudentTableSeeder
#### 执行单个填充
  php artisan db:seed--class=StudentTableSeeder

#### 执行所有填充
  php artisan db:seed
#### 创建中间件 （app/Http/Middleware  下）
  php artisan make:middlewareActivity

#### 创建队列（数据库）的表迁移（需要执行迁移才生效）
  php artisan queue:table
#### 创建队列类 （app/jobs下） ：
  php artisan make:jobSendEmail

#### 创建请求类（app/Http/Requests下）
  php artisan make:request CreateArticleRequest

### PHP Artisan 应用实例

#### 控制器 or Model

```
// 5.2版本创建一个空控制器
php artisan make:controller BlogController
// 创建Rest风格资源控制器
php artisan make:controller PhotoController --resource
// 指定创建位置 在app目录下创建TestController
php artisan make:controller App\TestController
// 指定路径创建
php artisan make:Model App\\Models\\User(linux or macOs 加上转义符)
// 数据迁移
php artisan migrate
```
#### 数据迁移（Migration）

```
// 创建迁移
php artisan make:migration create_users_table
// 指定路径
php artisan make:migration --path=app\providers create_users_table
// 一次性创建
// 下述命令会做两件事情：
// 在 app 目录下创建模型类 App\Post
// 创建用于创建 posts 表的迁移，该迁移文件位于 database/migrations 目录下。
php artisan make:model --migration Post

```

#### 数据填充（Seeder）

```
// 创建要填充的数据类
php artisan make:seeder UsersTableSeeder
// 数据填充（全部表）
php artisan db:seed
// 指定要填充的表
php artisan db:seed --class=UsersTableSeeder
```

#### 路由

```
// 查看所有路由
php artisan route:list
```

#### tinker命令插入单条数据

```
E:\opensource\blog>php artisan tinker
Psy Shell v0.7.2 (PHP 5.6.19 鈥?cli) by Justin Hileman
>>> $user = new App\User;
=> App\User {#628}
>>> $user->name = 'admin'
=> "admin"
>>> $user->email = 'fation@126.com'
=> "fation@126.com"
>>> $user->password = bcrypt('123456');
=> "$2y$10$kyCuwqSpzGTTZgAPMgCDgung9miGRygyCAIKHJhylYyW9osKKc3lu"
>>> $user->save();
"insert into `users` (`name`, `email`, `password`, `updated_at`, `created_at`) v
alues (?, ?, ?, ?, ?)"
=> true
>>> exit
Exit:  Goodbye.
```

#### Request请求，主要用于表单验证
```
php artisan make:request TagCreateRequest
```

创建的类存放在 app/Http/Requests 目录下

```PHP
<?php 
 
namespace App\Http\Requests;
 
use App\Http\Requests\Request;
 
class TagCreateRequest extends Request
{
 
    /**
     * Determine if the user is authorized to make this request.
     *
     * @return bool
     */
    public function authorize()
    {
        return true;
    }
 
    /**
     * Get the validation rules that apply to the request.
     *
     * @return array
     */ 
    public function rules()
    {
        return [
            'tag' => 'required|unique:tags,tag',
            'title' => 'required',
            'subtitle' => 'required',
            'layout' => 'required',
        ];
    }
}
```

使用时只需在对应的Controller方法里引入

```PHP
// 注意这里使用的是TagCreateRequest
public function store(TagCreateRequest $request)
{
    $tag = new Tag();
    foreach (array_keys($this->fields) as $field) {
        $tag->$field = $request->get($field);
    }
    $tag->save();
    return redirect('/admin/tag')
        ->withSuccess("The tag '$tag->tag' was created.");
}
```

#### 创建artisan命令行（laravel5.*版本）

```PHP
// 以下命令生成文件 app/Console/Commands/TopicMakeExcerptCommand.php
 
php artisan make:console TopicMakeExcerptCommand --command=topics:excerpt
//在 app/Console/Kernel.php 文件里面, 添加以下
protected $commands = [
        \App\Console\Commands\TopicMakeExcerptCommand::class,
    ];
//激活artisan命令行。
//在生成的TopicMakeExcerptCommand.php 文件, 修改以下区域
<?php
 
namespace App\Console\Commands;
 
use Illuminate\Console\Command;
 
class TopicMakeExcerptCommand extends Command
{
    /**
     * 1. 这里是命令行调用的名字, 如这里的: `topics:excerpt`, 
     * 命令行调用的时候就是 `php artisan topics:excerpt`
     *
     * @var string
     */
    protected $signature = 'topics:excerpt';
 
    /**
     * 2. 这里填写命令行的描述, 当执行 `php artisan` 时
     *   可以看得见.
     *
     * @var string
     */
    protected $description = '这里修改为命令行的描述';
 
    /**
     * Create a new command instance.
     *
     * @return void
     */
    public function __construct()
    {
        parent::__construct();
    }
 
    /**
     * 3. 这里是放要执行的代码, 如在我这个例子里面,
     *   生成摘要, 并保持.
     *
     * @return mixed
     */
    public function handle()
    {
        $topics = Topic::all();
        $transfer_count = 0;
 
        foreach ($topics as $topic) {
          if (empty($topic->excerpt))
          {
              $topic->excerpt = Topic::makeExcerpt($topic->body);
              $topic->save();
              $transfer_count++;
          }
        }
        $this->info("Transfer old data count: " . $transfer_count);
        $this->info("It's Done, have a good day.");
    }
}
```

```
// 命令行调用
php artisan topics:excerpt 
```


##命令列表基于 php artisan list命令

#### Usage:
    command [options] [arguments]

#### Options:
    -h, --help            Display help for the given command. When no command is given display help for the list command
    -q, --quiet           Do not output any message
    -V, --version         Display this application version
        --ansi|--no-ansi  Force (or disable --no-ansi) ANSI output
    -n, --no-interaction  Do not ask any interactive question
        --env[=ENV]       The environment the command should run under
    -v|vv|vvv, --verbose  Increase the verbosity of messages: 1 for normal output, 2 for more verbose output and 3 for debug

#### Available commands:
    clear-compiled        Remove the compiled class file
    completion            Dump the shell completion script
    db                    Start a new database CLI session
    down                  Put the application into maintenance / demo mode
    env                   Display the current framework environment
    help                  Display help for a command
    inspire               Display an inspiring quote
    list                  List commands
    migrate               Run the database migrations
    optimize              Cache the framework bootstrap files
    serve                 Serve the application on the PHP development server
    test                  Run the application tests
    tinker                Interact with your application
    up                    Bring the application out of maintenance mode
#### auth
    auth:clear-resets     Flush expired password reset tokens
#### breeze
    breeze:install        Install the Breeze controllers and resources
#### cache
    cache:clear           Flush the application cache
    cache:forget          Remove an item from the cache
    cache:table           Create a migration for the cache database table
#### config
    config:cache          Create a cache file for faster configuration loading
    config:clear          Remove the configuration cache file
#### db
    db:seed               Seed the database with records
    db:wipe               Drop all tables, views, and types
#### event
    event:cache           Discover and cache the application's events and listeners
    event:clear           Clear all cached events and listeners
    event:generate        Generate the missing events and listeners based on registration
    event:list            List the application's events and listeners
#### key
    key:generate          Set the application key
#### make
    make:cast             Create a new custom Eloquent cast class
    make:channel          Create a new channel class
    make:command          Create a new Artisan command
    make:component        Create a new view component class
    make:controller       Create a new controller class
    make:event            Create a new event class
    make:exception        Create a new custom exception class
    make:factory          Create a new model factory
    make:job              Create a new job class
    make:listener         Create a new event listener class
    make:mail             Create a new email class
    make:middleware       Create a new middleware class
    make:migration        Create a new migration file
    make:model            Create a new Eloquent model class
    make:notification     Create a new notification class
    make:observer         Create a new observer class
    make:policy           Create a new policy class
    make:provider         Create a new service provider class
    make:request          Create a new form request class
    make:resource         Create a new resource
    make:rule             Create a new validation rule
    make:seeder           Create a new seeder class
    make:test             Create a new test class
#### migrate
    migrate:fresh         Drop all tables and re-run all migrations
    migrate:install       Create the migration repository
    migrate:refresh       Reset and re-run all migrations
    migrate:reset         Rollback all database migrations
    migrate:rollback      Rollback the last database migration
    migrate:status        Show the status of each migration
#### model
    model:prune           Prune models that are no longer needed
#### notifications
    notifications:table   Create a migration for the notifications table
#### optimize
    optimize:clear        Remove the cached bootstrap files
#### package
    package:discover      Rebuild the cached package manifest
#### queue
    queue:batches-table   Create a migration for the batches database table
    queue:clear           Delete all of the jobs from the specified queue
    queue:failed          List all of the failed queue jobs
    queue:failed-table    Create a migration for the failed queue jobs database table
    queue:flush           Flush all of the failed queue jobs
    queue:forget          Delete a failed queue job
    queue:listen          Listen to a given queue
    queue:monitor         Monitor the size of the specified queues
    queue:prune-batches   Prune stale entries from the batches database
    queue:prune-failed    Prune stale entries from the failed jobs table
    queue:restart         Restart queue worker daemons after their current job
    queue:retry           Retry a failed queue job
    queue:retry-batch     Retry the failed jobs for a batch
    queue:table           Create a migration for the queue jobs database table
    queue:work            Start processing jobs on the queue as a daemon
#### route
    route:cache           Create a route cache file for faster route registration
    route:clear           Remove the route cache file
    route:list            List all registered routes
#### sail
    sail:install          Install Laravel Sail's default Docker Compose file
    sail:publish          Publish the Laravel Sail Docker files
#### schedule
    schedule:clear-cache  Delete the cached mutex files created by scheduler
    schedule:list         List the scheduled commands
    schedule:run          Run the scheduled commands
    schedule:test         Run a scheduled command
    schedule:work         Start the schedule worker
#### schema
    schema:dump           Dump the given database schema
#### session
    session:table         Create a migration for the session database table
#### storage
    storage:link          Create the symbolic links configured for the application
#### stub
    stub:publish          Publish all stubs that are available for customization
#### vendor
    vendor:publish        Publish any publishable assets from vendor packages
#### view
    view:cache            Compile all of the application's Blade templates
    view:clear            Clear all compiled view files
