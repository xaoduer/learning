# 技术栈
1.Docker  
 MySQL mybatis springboot 微信小程序  
 
2.云服务器  
 
# 解决痛点
1.卖家：卖家设置每一本书的价格区间，竞拍时间，比如1天内，以分钟为单位  

2.用户既可以是卖家也可以是买家  
用户表  
Table user :  
```
id 
wxid 
name(wxname) 微信名
sex 性别
mobile 手机号码 
email 邮箱
address  地址信息
ctime 
mtime
```

上架书单表  
Table book:  
```
userid 
id 
name 书名
amount 库存
min_prize 起拍价格  
max_prize  最高价格  
min_prize_span 最低竞价  
alive_time  有效时间，有效期到了自动交易或者终止交易  
state 状态，已售出、在售、已下架  
ctime
mtime;
```

派送地址信息表(买方)  
Table Address  
```
id 
userid 买方信息
mobile  收件人手机号
address 收件人地址
ctime 
mtime
```

买家购物车   
Table order  
```
id 
bookid 所购书籍，以";"间隔。如果用户选择了不同卖家的书籍，派单时自动按照卖方id分隔  
amount 数目
userid 买方信息
ctime 
mtime
```

派送订单表  
Table order  
```
id 
sellerid 卖方userid 冗余id，建立买家和卖家的联系
bookids 所购书籍，可以是多本书籍，但必须属于同一卖家，合并订单，方便凑单省运费 <bookid>*<count> "223124*2"
total_prize 总价
fee 手续费 2%
userid 买方信息
mobile  收件人手机号
address 收件人地址
state 交易状态：买家已拍、买家已付款、卖家已发货、卖家已提供物流信息、买家已签收、交易完成、交易存疑
tracking_number 快递单号
ctime 
mtime
```


3.买家可以自由搭配同一个卖家的所有或者部分书籍一起竞价，节省邮费  
