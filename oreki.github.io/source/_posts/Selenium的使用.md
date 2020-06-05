---
title: Selenium的使用
toc: true
mathjx: true
cover: /"Selenium的使用"/head.png
date: 2019-01-28 16:18:24
update:
tags: ['Selenuim']
categories: Selenuim
---
## Selenium的使用

### 模拟谷歌浏览器访问百度首页，并输入python关键字搜索
~~~
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.support.wait import WebDriverWait

#初始化一个浏览器（如：谷歌，使用Chrome需安装chromedriver）
driver = webdriver.Chrome()
#driver = webdriver.PhantomJS() #无界面浏览器
try:
    #请求网页
    driver.get("https://www.baidu.com")
    #查找id值为kw的节点对象（搜索输入框）
    input = driver.find_element_by_id("kw")
    #模拟键盘输入字串内容
    input.send_keys("python")
    #模拟键盘点击回车键
    input.send_keys(Keys.ENTER)
    #显式等待,最长10秒
    wait = WebDriverWait(driver,10)
    #等待条件：10秒内必须有个id属性值为content_left的节点加载出来，否则抛异常。
    wait.until(EC.presence_of_element_located((By.ID,'content_left')))
    # 输出响应信息
    print(driver.current_url)
    print(driver.get_cookies())
    print(driver.page_source)
finally:
    #关闭浏览器
    #driver.close()
    pass
~~~

### 声明浏览器对象

~~~
from selenium import webdriver

driver = webdriver.Chrome()  #谷歌 需：ChromeDriver驱动
driver = webdriver.FireFox() #火狐 需：GeckoDriver驱动
driver = webdriver.Edge()  
driver = webdriver.Safari()  
driver = webdriver.PhantomJS() #无界面浏览器
~~~

### 访问页面
~~~
from selenium import webdriver

driver = webdriver.Chrome()
#driver = webdriver.PhantomJS()
driver.get("http://www.taobao.com")
print(driver.page_source)
#driver.close()
~~~

### 查找节点
#### 获取单个节点的方法
>find_element_by_id()
find_element_by_name()
find_element_by_xpath()
find_element_by_link_text()
find_element_by_partial_link_text()
find_element_by_tag_name()
find_element_by_class_name()
find_element_by_css_seletor()

~~~
from selenium import webdriver
from selenium.webdriver.common.by import By

#创建浏览器对象
driver = webdriver.Chrome()
#driver = webdriver.PhantomJS()
driver.get("http://www.taobao.com")
#下面都是获取id属性值为q的节点对象
input = driver.find_element_by_id("q")
print(input)

input = driver.find_element_by_css_selector("#q")
print(input)

input = driver.find_element_by_xpath("//*[@id='q']")
print(input)

#效果同上
input = driver.find_element(By.ID,"q")
print(input)

#driver.close()
~~~

#### 获取多个节点的方法
>find_elements_by_id()
find_elements_by_name()
find_elements_by_xpath()
find_elements_by_link_text()
find_elements_by_partial_link_text()
find_elements_by_tag_name()
find_elements_by_class_name()
find_elements_by_css_seletor()
~~~
from selenium import webdriver
import time

#创建浏览器对象
driver = webdriver.Chrome()
#driver = webdriver.PhantomJS()
driver.get("http://www.taobao.com")
#下面都是获取id属性值为q的节点对象
input = driver.find_element_by_id("q")
#模拟键盘输入iphone
input.send_keys('iphone')
time.sleep(3)
#清空输入框
input.clear()
#模拟键盘输入iPad
input.send_keys('iPad')
#获取搜索按钮节点
botton = driver.find_element_by_class_name("btn-search")
#触发点击动作
botton.click()

#driver.close()
~~~

#### 动态链
ActionChains是一种自动化低级别交互的方法，如鼠标移动，鼠标按钮操作，按键操作和上下文菜单交互。这对于执行更复杂的操作（如悬停和拖放）很有用.
>move_to_element（to_element ）-- 将鼠标移到元素的中间
move_by_offset（xoffset，yoffset ）-- 将鼠标移至当前鼠标位置的偏移量
drag_and_drop（源，目标）-- 然后移动到目标元素并释放鼠标按钮。
pause（秒）-- 以秒为单位暂停指定持续时间的所有输入
perform（）-- 执行所有存储的操作。
release（on_element = None ）释放元素上的一个持有鼠标按钮。
reset_actions（）-- 清除已存储在远程端的操作。
send_keys（* keys_to_send ）-- 将键发送到当前的焦点元素。
send_keys_to_element（element，* keys_to_send ）-- 将键发送到一个元素。

~~~
#创建浏览器对象
driver = webdriver.Chrome()
#加载指定url地址
url = 'http://www.runoob.com/try/try.php?filename=jqueryui-api-droppable'
driver.get(url)
# 切换Frame窗口    
driver.switch_to.frame('iframeResult')
#获取两个div节点对象
source = driver.find_element_by_css_selector("#draggable")
target = driver.find_element_by_css_selector("#droppable")
#创建一个动作链对象
actions = ActionChains(driver)
#将一个拖拽操作添加到动作链队列中
actions.drag_and_drop(source,target)
time.sleep(3)
#执行所有存储的操作（顺序被触发）
actions.perform()
#driver.close()
~~~

#### 执行JavaScript
~~~
from selenium import webdriver

#创建浏览器对象
driver = webdriver.Chrome()
#加载指定url地址
driver.get("https://www.zhihu.com/explore")
#执行javascript程序将页面滚动移至底部
driver.execute_script('window.scrollTo(0,document.body.scrollHeight)')
#执行javascript实现一个弹框操作
driver.execute_script('window.alert("Hello Selenium!")')

#driver.close()
~~~

#### 获取节点信息
~~~
from selenium import webdriver
from selenium.webdriver import ActionChains

#创建浏览器对象
driver = webdriver.Chrome()
#加载请求指定url地址
driver.get("https://www.zhihu.com/explore")
#获取id属性值为zh-top-link-logo的节点（logo）
logo = driver.find_element_by_id("zh-top-link-logo")
print(logo) #输出节点对象
print(logo.get_attribute('class')) #节点的class属性值
#获取id属性值为zu-top-add-question节点（提问按钮）
input = driver.find_element_by_id("zu-top-add-question")
print(input.text) #获取节点间内容
print(input.id)  #获取id属性值
print(input.location) #节点在页面中的相对位置
print(input.tag_name) #节点标签名称
print(input.size)     #获取节点的大小
#driver.close()
~~~

#### 切换Frame
我们可以使用switch_to.frame()来切换Frame界面

### 延迟等待
浏览器加载网页是需要时间的，Selenium也不例外，若要获取完整网页内容，就要延时等待。
在Selenium中延迟等待方式有两种：一种是隐式等待，一种是显式等待（推荐）

~~~
rom selenium import webdriver

#创建浏览器对象
driver = webdriver.Chrome()
#使用隐式等待(固定时间)
driver.implicitly_wait(2)
#加载请求指定url地址
driver.get("https://www.zhihu.com/explore")
#获取节点    
input = driver.find_element_by_id("zu-top-add-question")
print(input.text) #获取节点间内容

#driver.close()
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support import expected_conditions as EC
from selenium.webdriver.support.wait import WebDriverWait

#创建浏览器对象
driver = webdriver.Chrome()
#加载请求指定url地址
driver.get("https://www.zhihu.com/explore")
#显式等待,最长10秒
wait = WebDriverWait(driver,10)
#等待条件：10秒内必须有个id属性值为zu-top-add-question的节点加载出来，否则抛异常。
input = wait.until(EC.presence_of_element_located((By.ID,'zu-top-add-question')))
print(input.text) #获取节点间内容
#driver.close()
~~~

### 前进和后退
~~~
from selenium import webdriver
import time

#创建浏览器对象
driver = webdriver.Chrome()
#加载请求指定url地址
driver.get("https://www.baidu.com")
driver.get("https://www.taobao.com")
driver.get("https://www.jd.com")
time.sleep(2)
driver.back() #后退
time.sleep(2) #前进
driver.forward()
#driver.close()
~~~

### cookies
~~~
from selenium import webdriver
from selenium.webdriver import ActionChains

#创建浏览器对象
driver = webdriver.Chrome()
#加载请求指定url地址
driver.get("https://www.zhihu.com/explore")
print(driver.get_cookies())
driver.add_cookie({'name':'namne','domain':'www.zhihu.com','value':'zhangsan'})
print(driver.get_cookies())
driver.delete_all_cookies()
print(driver.get_cookies())
#driver.close()
~~~

### 选项卡管理
~~~
from selenium import webdriver
import time

#创建浏览器对象
driver = webdriver.Chrome()
#加载请求指定url地址
driver.get("https://www.baidu.com")
#使用JavaScript开启一个新的选型卡
driver.execute_script('window.open()')
print(driver.window_handles)
#切换到第二个选项卡，并打开url地址
driver.switch_to_window(driver.window_handles[1])
driver.get("https://www.taobao.com")
time.sleep(2)
#切换到第一个选项卡，并打开url地址
driver.switch_to_window(driver.window_handles[0])
driver.get("https://www.jd.com")
#driver.close()
~~~

### 异常处理
~~~
from selenium import webdriver
from selenium.common.exceptions import TimeoutException,NoSuchElementException

#创建浏览器对象
driver = webdriver.Chrome()
try:
    #加载请求指定url地址
    driver.get("https://www.baidu.com")
except TimeoutException:
    print('Time Out')

try:
    #加载请求指定url地址
    driver.find_element_by_id("demo")
except NoSuchElementException:
    print('No Element')
finally:
    #driver.close()
    pass
~~~
