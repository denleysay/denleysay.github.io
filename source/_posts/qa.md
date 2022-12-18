---
layout: post
title: 问题汇总
date: 2022-08-27 09:31
categories:
  - 技术
---

经历的各种问题汇总
<!-- More -->

* chrome打开网页地址栏提示Your connection to this site is not secure，可[参考](https://www.cnblogs.com/csn0721/p/16383521.html)

# Rails

* 对任意第三方rails程序，不要直接使用 `bundle install`, 最好使用`bundle update`

* Rails中出现`incompatible encoding regexp match (UTF-8 regexp with ASCII-8BIT string)`时
  原因是httparty版本问题，执行：`bundle update`

* Rails服务停止：`kill -9 $(ps -A | grep ruby)`

* 如果要测试redirect_to后的页面内容，要先执行`follow_redirect!`, 如果中间有几个重定向，则同样需要执行几个`follow_redirect!`,如无法确定当前页面，可以通过以下命令先查看其内容：
  ```ruby
  assert_equal '', body
  ```
* rails只有`has_and_belongs_to_many`关系，没有has_many_and_belongs_to关系。

* Micropost/Tag互相建立has_and_belongs_to_many关系后,用“micropost[tag_ids][]”接收输入时，要在micropost_params中增加:tag_ids:[];
  详情搜索：rails 健壮参数

* Micropost/Tag互相建立has_and_belongs_to_many关系后，还要通过以下命令建立中间表：`rails g migration CreateJoinTable micropost tag`

* 在User/Micropost的`has_many microposts `一对多关系中,完整描述是：
  ```ruby
  has_many microposts, class_name:”Micropost“, foreigh_key:”user_id“
  ```
  最好在最后还加上：`dependent: :destroy`

* Rails controller中render渲染的template肯定不是partial, 而view中由partial渲染的template内部不能再有render渲染

* 当有多个migration对同一表(user)结构进行修改时，建议后续表作初始化操作时使用如下方式：
  ```ruby
  User.connection.schema_cache.clear!
  User.reset_column_information
  ```
  此“初始化操作”是否应该做完就删除，因为重新做: r db:drop ==> db:migrate 时会失败；

* Rails 5为什么不缺省打开`gem therubyracer`
  更不错的可选方案nodejs, 在ubuntu上安装如下：`sudo apt-get install nodejs`

## 使用Postgre数据库
在`Gemfile`中增加`gem 'pg'`, 再安装其依赖库:

* For Ubuntu systems: sudo apt-get install libpq-dev
* On Red Hat Linux (RHEL) systems: yum install postgresql-devel
* For Mac Homebrew: brew install postgresql
* For Mac MacPorts PostgreSQL: gem install pg – –with-pg-config=/opt/local/lib/postgresql[version number]/bin/pg_config
* For OpenSuse: zypper in postgresql-devel

## 使用二级域名

```
location /e_car {
    root /path/to/public;
    proxy_pass http://localhost:3003/;
}
```
注意，这时3003后的‘/'不能省略，否则意思完全不同

2. 在config/environments/production中设置：
```ruby
  config.relative_url_root = '/e_car'
```

3. 在config/routes中设置scope:
```ruby
scope 'e_car' do
    ......
end
```

## 关闭生成器的自动功能
在`config/application.rb`中增加:

```ruby
    config.generators do |g|
      g.assets false
      g.helper false
      g.test_framework false
    end
```

## 找不到Gem
现象：用到`assert_template`时，如果只是在`group :test`中加入`gem rails-controller-testing`，还是会出现错误提示
> 此Gem找不到

方法：加入到`group :development, :test`中即可

注；此时加回到`group :test`也能通过。

## 测试失败
现象：运行以下测试时失败
```ruby
  relation = UserRelation.new(follower_id:1, followed_id:2)
  assert relation.valid?
```

原因：因为UserRelation模型中指定了它与user的关系。
```ruby
  belongs_to :follower, class_name:“User”
  belongs_to :followed, class_name:”User“
```

方法：在user.yml中指定对应id的user

## 路由错误
现象：`ActionController::RoutingError (No route matches [GET] "/assets/application-XXXX`

原因：production环境下rails的安全控制

方法：增加环境变量`export RAILS_SERVE_STATIC_FILES=xxx(任意值）`

## 令牌无效
现象：`ActionController::InvalidAuthenticityToken`

方法：通过在config/envirments/production.rb中增加：
```
config.action_controller.allow_forgery_protection = false
```

## 参考
* [ActionController::InvalidAuthenticityToken解决办法](http://blog.csdn.net/iam_song/article/details/7688631)
* [Rails 中消失的 CSRF token](https://ruby-china.org/topics/21821)
