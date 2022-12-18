---
title:  "Ruby单元测试总结"
date:   2017-04-08 9:00:00
categories: 
- 技术
- Ruby
tags: [CI]
---

Ruby中最简单的单元测试操作指南

<!-- More -->

测试源文件: `people.rb`

```ruby
class People
  def name
  	"Hello"
  end
end
```

使用老版本的单元测试工具：Unit Test

```ruby
require 'test/unit'
require_relative 'people'

class TestPeople < Test::Unit::TestCase
  def setup
    @people = People.new
  end
  
  def test_name
  	assert_equal "Hello", @people.name
  end
end
```

使用新版本的测试工具：MiniTest

```ruby

require 'minitest/autorun'
require_relative 'people'

class TestPeople < Minitest::Test
  def setup
    @people = People.new
  end
  
  def test_name
  	assert_equal "Hello", @people.name
  end
end
```

可参考[测试指南](http://chloerei.com/2015/10/26/testing-guide)