定位一组对象
============

场景
----
webdriver除了可以很方便的使用findElement方法来定位某个特定的对象，还可以使用findElements去定位一组对象。

定位一组对象一般用于以下场景：

* 批量操作对象，比如将页面上所有的checkbox都勾上
* 先获取一组对象，再在这组对象中过滤出需要具体定位的一些对象。比如定位出页面上所有的checkbox，然后选择最后一个

代码
----

### checkbox.html
```
	<html>
		<head>
			<meta http-equiv="content-type" content="text/html;charset=utf-8" />
			<title>Checkbox</title>
			<script type="text/javascript" async="" src="https://ajax.googleapis.com/ajax/libs/jquery/1.9.1/jquery.min.js"></script>
			<link href="http://netdna.bootstrapcdn.com/twitter-bootstrap/2.3.2/css/bootstrap-combined.min.css" rel="stylesheet" />
			<script src="http://netdna.bootstrapcdn.com/twitter-bootstrap/2.3.2/js/bootstrap.min.js"></script>
		</head>
		<body>
			<h3>checkbox</h3>
			<div class="well">
				<form class="form-horizontal">
					<div class="control-group">
						<label class="control-label" for="c1">checkbox1</label>
						<div class="controls">
							<input type="checkbox" id="c1" />
						</div>
					</div>
					<div class="control-group">
						<label class="control-label" for="c2">checkbox2</label>
						<div class="controls">
							<input type="checkbox" id="c2" />
						</div>
					</div>
					<div class="control-group">
						<label class="control-label" for="c3">checkbox3</label>
						<div class="controls">
							<input type="checkbox" id="c3" />
						</div>
					</div>						
					<div class="control-group">
						<label class="control-label" for="r">radio</label>
						<div class="controls">
							<input type="radio" id="r" />
						</div>
					</div>						
				</form>
			</div>
		</body>
	</html>
```

### find_element.py
```
# -*- coding: utf-8 -*-
from selenium import webdriver
import time
import os

dr = webdriver.Firefox()
file_path =  'file:///' + os.path.abspath('checkbox.html')
dr.get(file_path)

# 选择所有的checkbox并全部勾上
checkboxes = dr.find_elements_by_css_selector('input[type=checkbox]')
for checkbox in checkboxes:
	checkbox.click()
time.sleep(1)
dr.refresh()
time.sleep(2)
#选择某一个元素，可以这样做
checkboxes[0].click()
# 打印当前页面上有多少个checkbox
print len(dr.find_elements_by_css_selector('input[type=checkbox]'))

# 选择页面上所有的input，然后从中过滤出所有的checkbox并勾选之
inputs = dr.find_elements_by_tag_name('input')
for input in inputs:
	if input.get_attribute('type') == 'checkbox':
		input.click()

time.sleep(1)

# 把页面上最后1个checkbox的勾给去掉
dr.find_elements_by_css_selector('input[type=checkbox]').pop().click()

time.sleep(1)

dr.quit()


```

