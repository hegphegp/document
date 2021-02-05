## 天勤API

### 1.0 一些问题
##### 1.1 天勤API是否有获取所有期货编号的代码例子
##### 1.2 如果不是模拟，怎么填写账号？账号哪里来？快期网站注册的账号有什么用？   

```python
from tqsdk import TqApi, TqAccount, TargetPosTask

api = TqApi(TqAccount("H海通期货", "4003242", "123456"))       # 创建 TqApi 实例, 指定交易账户
quote = api.get_quote("SHFE.cu1812")
print (quote["datetime"], quote["last_price"])
```

##### 1.3
##### 1.4
##### 1.5
##### 1.6
##### 1.7
##### 1.8
##### 1.9
##### 1.10
##### 1.11
##### 1.12
##### 1.13
##### 1.14
##### 1.15
##### 1.16
##### 1.17
##### 1.18
##### 1.19
##### 1.20

### 2.0 安装或者升级天勤的SDK
```
pip install tqsdk
pip install --upgrade tqsdk --user
```

### 3.0 获取数据
#### 3.1 获取实时数据
#### 3.1.1 获取单个合约的实时数据
```python
from tqsdk import TqApi, TqSim

api = TqApi(TqSim())
quote = api.get_quote("SHFE.cu1812")

while True:
    # 调用 wait_update 等待业务信息发生变化，例如: 行情发生变化, 委托单状态变化, 发生成交等等
    api.wait_update()
    # 注意：其他合约的行情的更新也会触发业务信息变化，因此这里可能会将同一笔行情输出多次
    print(quote["datetime"], quote["last_price"])
```

#### 3.1.2 获取多个合约的实时数据
```python
from tqsdk import TqApi, TargetPosTask, TqSim

api = TqApi(TqSim())
q_1910 = api.get_quote("SHFE.rb1910")                         # 订阅近月合约行情
q_2001 = api.get_quote("SHFE.rb2001")                         # 订阅远月合约行情

print (q_1910["datetime"], q_1910["last_price"])              # 打印是为了说明代码能执行，并且有输出
print (q_2001["datetime"], q_2001["last_price"])              # 打印是为了说明代码能执行，并且有输出

while True:
    api.wait_update()                                           # 等待数据更新
    print(q_1910["datetime"], q_1910["last_price"])
    print(q_2001["datetime"], q_2001["last_price"])
```



```python
from tqsdk import TqApi, TargetPosTask, TqSim

api = TqApi(TqSim())
q_1910 = api.get_quote("SHFE.rb1910")                         # 订阅近月合约行情
t_1910 = TargetPosTask(api, "SHFE.rb1910")                    # 创建近月合约调仓工具
q_2001 = api.get_quote("SHFE.rb2001")                         # 订阅远月合约行情
t_2001 = TargetPosTask(api, "SHFE.rb2001")                    # 创建远月合约调仓工具

print (q_1910["datetime"], q_1910["last_price"])              # 打印是为了说明代码能执行，并且有输出
print (q_2001["datetime"], q_2001["last_price"])              # 打印是为了说明代码能执行，并且有输出

while True:
  api.wait_update()                                           # 等待数据更新
  spread = q_1910["last_price"] - q_2001["last_price"]        # 计算近月合约-远月合约价差
  print("当前价差:", spread)
  if spread > 250:
    print("价差过高: 空近月，多远月")
    t_1910.set_target_volume(-1)                              # 要求把1910合约调整为空头1手
    t_2001.set_target_volume(1)                               # 要求把2001合约调整为多头1手
  elif spread < 200:
    print("价差回复: 清空持仓")                                  # 要求把 1910 和 2001合约都调整为不持仓
    t_1910.set_target_volume(0)
    t_2001.set_target_volume(0)
```