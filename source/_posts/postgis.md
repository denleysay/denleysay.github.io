---
title:  "PostGIS在Rails中的应用"
date:   2015-11-13 9:00:00
categories: 
- 技术
- Rails
tags: [Database]
---

Rails中使用支持PostGIS的Postgres数据库

<!-- More -->

# 修改
1. Gemfile文件
```ruby
group :production do
  gem 'pg'
  gem 'activerecord-postgis-adapter'
end
```

2. config/database.yml文件
```ruby
production:
  adapter: postgis
  encoding: unicode
  # For details on connection pooling, see rails configuration guide
  # http://guides.rubyonrails.org/configuring.html#database-pooling
  pool: <%= ENV.fetch("RAILS_MAX_THREADS") { 5 } %>
  database: e_car
  username: denley
  password: <%= ENV['DENLEY_STUDIO_DATABASE_PASSWORD'] %>
```

# 执行
1. brew install postgis

2. 创建数据库
```bash
r db:create RAILS_ENV=production
r db:migrate RAILS_ENV=production
```

如执行第1步时出现错误：`Must be superuser to create this extension. : CREATE EXTENSION IF NOT EXISTS postgis `,
则给用户赋于超级用户权限：createuser --superuser <user_name>(比如这里是denley);

注：因为使用起来有点难度，目前采用了geocoder gem.