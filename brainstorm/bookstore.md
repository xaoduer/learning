# 技术栈
先做原型  
1.Docker  
 MySQL mybatis springboot 微信小程序  
 
2.云服务器  
 
# 解决痛点
1.卖家：卖家设置每一本书的价格区间，竞拍时间，比如1天内，以分钟为单位。针对2手书价格界定不清晰，让买卖双方都能达成一个合理的价格   

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
category 类别 计算机
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

买家竞价单   
Table cart  
```
id 
bookid 所购书籍，以";"间隔。如果用户选择了不同卖家的书籍，派单时自动按照卖方id分隔  
amount 数目 保留
userid 买方信息
prize 参与价格
state 竞价状态：买家报价、卖家拒绝、卖家同意、竞价失败（有更高价）、买家取消
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

# 接口设计
## 1.首页：并随机显示10本上架书籍  
	按拍卖结束时间倒计时，排序显示  
	有类别tab，用户可点击类别筛选
GET 
```
select * from book   order by rand() limit 10;
select * from book where category='computer'  order by rand() limit 10;

mysql> select max(id),min(id) into @M,@N from book ;
set @X= floor((@M-@N+1)*rand() + @N);
select * from book where id >= @X limit 1;
```
或者 按照活动倒计时间排序显示  

## 2.点击详情页面  
```
GET 
入参 id 
出参 select * from book where id = ''
```

## 3.参与竞价
等参与的最后一单竞价书籍结束后再派送订单  
```
POST
入参 bookid userid prize 
bookid -> sellerid  userid 
insert (...) into cart ...
```

## 4.取消竞价
```
POST
入参 id
update   cart where id = '' set state = 'cancel'
```

## 5.确认竞价
根据userid查询是否还存在未结束的竞价订单，存在则提示需要先取消  
不存在则更新状态，生成订单，等待卖家确认，发货。

## 6.卖家发货后回填订单号
通过快递单号调用接口查询订单信息  

  
## 设计中...

## 暂不支持相同的书籍多本竞价，先实现原型