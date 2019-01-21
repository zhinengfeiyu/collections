- 不同的应用如果有相同id的NotificationChannel，互不影响；
    同一应用覆盖安装，不能修改NotificationChannel的id，因为修改id之后，会新增一个channel，而不是修改原来的channel；
    但是覆盖安装修改同一channelId的channelName是可以成功的，修改Importance是无法实现的
- 如果使用系统样式，必须调用setLargeIcon方法，否则展开时看到的左侧图标是一团灰，不会默认使用smallIcon；
- 必须调用setSmallIcon方法，否则不会出现通知，或者发生崩溃；
- 调用了setAutoCancel(true)以后，如果没有设置PendingIntent，点击通知不会自动消失；
- 最后调用的notify方法，如果id参数对应的Notification已存在，会替换掉已存在的这个Notification
- 判断PendingIntent是不是同一个，系统根据requestCode和Intent两个参数，只要有一个参数不同即不是同一个
   PendingIntent，注意判断Intent是否相同，不考虑extra数据，只考虑目标class等其他数据
- 关于Notification分组：如果有多个通知设置了相同的group，但是没有groupSummary为true的通知，这里所有的通知设置的group不起效果;
    如果相同group的通知只有一个，同时还有一个groupSummary为true的通知，这里的group同样	不起作用