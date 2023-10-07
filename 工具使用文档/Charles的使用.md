### Charles使用技巧
- 右键某一个请求，点击Focus，就可以在Structure里面把这个host单独列出来，其他请求折叠到Other Hosts下面
- Command+F可以在当前Session里面按关键词查找，可以指定是在请求路径、请求体或者返回体里面查找
- Sequence底部可执行临时过滤，勾选Focused可缩小过滤范围
- 模拟请求错误：右键某个请求，点击Black List，可让其返回404/403。但是这里把query参数也带上了，如果需要忽略后面那堆，还要在Tools->Black List里再次设置
- Rewrite功能比较多，可以拦截修改请求部分或返回部分。修改返回的数据：Type选择Body，Where选择Response。
  如果要产生map local的效果，Match部分不填，Replace部分把返回的完整json串填上；
  也可以通过正则或不通过正则修改某个返回参数值，使用"XX":\s*XX的正则匹配替换某个键值对，这时需要把Regex勾选上；
  修改请求的参数：Type选择Add/Modify/Remove Query Param，这时把Name和Value同时填上，Regex不需要勾选。这个修改请求参数的操作可以用Compose替代
- 修改请求参数后重发一次：单击顶部中间的Compose按钮，或者右键选择Compose，可修改请求路径或参数，点击Execute再次发送，点击Revert还原。
  这个带钢笔图标的Compose面板使用后会继续保留，后续还可以继续修改重发
- 直接重发：右键Repeat，或单击顶部中间的重试图标
- 查看返回结果需要顶部选中Contents标签，底部常用3种查看模式Text、JSON和JSON Text，如果数据过多查看完整结构可以切换到JSON标签
