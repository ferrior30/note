消息队列MQ
---
MQ特点
MQ是消费-生产者模型的一个典型的代表，一端往消息队列中不断写入消息，而另一端则可以读取或者订阅队列中的消息。MQ和JMS类似，但不同的是JMS是SUN JAVA消息中间件服务的一个标准和API定义，而MQ则是遵循了AMQP协议的具体实现和产品。
使用场景
在项目中，将一些无需即时返回且耗时的操作提取出来，进行了异步处理，而这种异步处理的方式大大的节省了服务器的请求响应时间，从而提高了系统的吞吐量。
1.背景

RabbitMQ是一个由erlang开发的AMQP(Advanved Message Queue)的开源实现。

2.应用场景

2.1异步处理
2.2应用解耦

场景：双11是购物狂节,用户下单后,订单系统需要通知库存系统,传统的做法就是订单系统调用库存系统的接口. 
这里写图片描述 
这种做法有一个缺点:

当库存系统出现故障时,订单就会失败。(这样马云将少赚好多好多钱^ ^)
订单系统和库存系统高耦合. 
引入消息队列 
这里写图片描述

订单系统:用户下单后,订单系统完成持久化处理,将消息写入消息队列,返回用户订单下单成功。

库存系统:订阅下单的消息,获取下单消息,进行库操作。 
就算库存系统出现故障,消息队列也能保证消息的可靠投递,不会导致消息丢失(马云这下高兴了).
流量削峰

流量削峰一般在秒杀活动中应用广泛 
场景:秒杀活动，一般会因为流量过大，导致应用挂掉,为了解决这个问题，一般在应用前端加入消息队列。 
作用: 
1.可以控制活动人数，超过此一定阀值的订单直接丢弃(我为什么秒杀一次都没有成功过呢^^) 
2.可以缓解短时间的高流量压垮应用(应用程序按自己的最大处理能力获取订单) 
这里写图片描述
1.用户的请求,服务器收到之后,首先写入消息队列,加入消息队列长度超过最大值,则直接抛弃用户请求或跳转到错误页面. 
2.秒杀业务根据消息队列中的请求信息，再做后续处理.