

### 1.接收电子小票流程

接口：POST: /api/v1/ticket

Body：

```json
{
    "ticket":
    {
        "ticket_id": "0633231421031618453247951750022",
        "ticket_no": "0633231421031618453247951750020",
        "start_time": "2021-03-16 18:45:29",
        "end_time": "2021-03-16 18:45:29",
        "upload_time": "2021-03-16 18:45:33",
        "bus_date": "2021-03-16",
        "pos":
        {
            "id": "4467565903218929665",
            "code": "11047059",
            "device_id": "",
            "device_code": ""
        },
        "operator":
        {
            "id": "4380287224818696193",
            "loginId": "4066",
            "name": "\u6768\u5c11\u82f1",
            "code": "4066",
            "login_time": ""
        },
        "amounts":
        {
            "taxAmount": 0,
            "gross_amount": -33,
            "pay_amount": -33,
            "net_amount": -33,
            "discount_amount": 0,
            "removezero_amount": 0,
            "rounding": 0,
            "overflow_amount": 0,
            "changeAmount": 0,
            "serviceFee": 0,
            "tip": 0
        },
        "takemealNumber": "6",
        "qty": -1,
        "status": "REFUND",
        "refundInfo":
        {
            "refund_id": "0633231421031618453247951750018",
            "ref_ticket_id": "0633231421031616071539251750012",
            "ref_ticket_no": "0633231421031616071539251750012",
            "refund_reason": ""
        },
        "channel":
        {
            "source": "POS",
            "deviceType": "PAD",
            "orderType": "DINEIN",
            "deliveryType": "REALTIME",
            "tpName": "INVALID",
            "code": "10005",
            "id": "4383963285460877313",
            "mapping_code": "pos"
        },
        "products":
        [
            {
                "id": "4454238806391914497",
                "name": "\u575a\u679c\u8c46",
                "code": "4070190067",
                "seq_id": 1,
                "price": 33,
                "amount": -33,
                "qty": -1,
                "discount_amount": 0,
                "type": "",
                "accessories": null,
                "combo_items": null,
                "remark": "",
                "skuRemark":
                [
                    {
                        "name":
                        {
                            "id": "",
                            "code": "10003",
                            "name": "\u725b\u5976"
                        },
                        "values":
                        {
                            "code": "1000301",
                            "name": "\u5168\u8102\u725b\u5976"
                        }
                    },
                    {
                        "name":
                        {
                            "id": "",
                            "code": "10001",
                            "name": "\u610f\u5f0f\u5496\u5561\u8c46"
                        },
                        "values":
                        {
                            "code": "1000102",
                            "name": "\u67d1\u6a58\u9999"
                        }
                    },
                    {
                        "name":
                        {
                            "id": "",
                            "code": "10004",
                            "name": "\u676f\u578b"
                        },
                        "values":
                        {
                            "code": "1000401",
                            "name": "\u6807\u51c6\u676f"
                        }
                    }
                ],
                "taxAmount": 0,
                "category_id": "4373443971733520385",
                "category_name": "\u539f\u6750\u6599-\u751f\u8c46-\u56fd\u5bb6\u00b7\u4e2d\u56fd"
            }
        ],
        "payments":
        [
            {
                "id": "4380223798482305025",
                "seq_id": "1",
                "name": "\u73b0\u91d1",
                "pay_amount": -33,
                "receivable": -33,
                "change": 0,
                "overflow": 0,
                "rounding": 0,
                "pay_time": "2021-03-16 18:45:29",
                "trans_code": "CASH",
                "tpTransactionNo": ""
            }
        ],
        "table":
        {
            "id": "",
            "zone_id": "",
            "tableNo": "",
            "people": 0,
            "temporary": false
        },
        "people": 1,
        "room_no": "-1",
        "order_time_type": "BFAST",
        "shiftNumber": "\u6768\u5c11\u82f1",
        "taxList":
        [
            {
                "amount": 0,
                "subTotal": -33,
                "code": "3070401000000000000",
                "name": "\u9910\u996e\u670d\u52a1"
            }
        ],
        "takeaway_info":
        {
            "order_method": "",
            "tp_order_id": "",
            "order_time": "",
            "description": "",
            "consignee": "",
            "phone_list": null,
            "tp": "",
            "source": "",
            "source_order_id": "",
            "day_seq": "",
            "delivery_type": 0,
            "delivery_name": "",
            "delivery_poi_address": "",
            "invoice_title": "",
            "waiting_time": "",
            "tableware_num": 0,
            "send_fee": 0,
            "package_fee": 0,
            "delivery_time": "",
            "take_meal_sn": "",
            "partnerPlatformId": 0,
            "partnerPlatformName": "",
            "wxName": "",
            "isHighPriority": false,
            "takeoutType": "",
            "originalOrderNo": ""
        },
        "store":
        {
            "id": "4374823993522257921",
            "code": "MD031",
            "secondCode": "",
            "companyId": "",
            "partnerId": "2",
            "scopeId": "0",
            "branchId": "0"
        },
        "transaction_no": "0633231421031618453247951750018"
    }
}
```

返回：



报表接口使用的全是Grpc接口，

查询接口使用的是Http接口





### 2.总处理时序图:

```mermaid
sequenceDiagram
	Title: 电子小票处理时序图
    participant 客户端
    participant SalesReport
    participant 数据库
    participant NSQ队列
    
    Note over 客户端,NSQ队列: 下面执行接收电子小票流程
    客户端 ->> SalesReport:1.发起POST请求（eticket）
    Note right of 客户端:POST: /api/v1/ticket
    SalesReport ->> 数据库: 2.组装时间日期字段，将电子小票数据存储起来(存储的时候，根据id查询是否已经存在了)
    Note right of SalesReport: SalesTicketContent
    SalesReport ->> NSQ队列: 3.发布新主题WaitCheckTopic(ticketID)到消息队列
    SalesReport -->> 客户端: 4.返回存起来的电子小票ID
    
    Note over 客户端,NSQ队列: 下面开始执行电子小票验证
    NSQ队列 ->> SalesReport: 5.接收到WaitCheckTopic(ticketID)主题数据,处理函数:handleWaitCheckTicket
    SalesReport ->> 数据库: 6.通过ticketID查询电子小票数据并返回
    Note right of SalesReport: SalesTicketContent
    SalesReport ->> SalesReport: 7.验证电子小票si.Status的状态是不是init（新增）,并开始一系列验证流程：_validateTicket
    SalesReport ->> 数据库: 8.验证通过后，将验证状态ProcessStatus改为已验证，并更新数据库
    SalesReport ->> NSQ队列: 9.发布新主题折扣分摊DiscountApportionTopic(ticketID)到消息队列
    
    Note over 客户端,NSQ队列: 下面开始执行折扣分摊验证
		NSQ队列 ->> SalesReport: 10.接收到DiscountApportionTopic(ticketID)主题数据，处理函数:handleDiscountApportion
		SalesReport ->> 数据库: 11.通过ticketID查询电子小票数据并返回
    Note right of SalesReport: SalesTicketContent
    SalesReport ->> SalesReport: 12.验证电子小票si.ProcessStatus的状态是不是验证成功，并判断ticket.Products>0,然后对想拍进行排序，并开始折扣分摊计算，处理函数：DiscountApportion
    SalesReport ->> 数据库: 13.折扣分摊通过后，将验证状态ProcessStatus改为折扣分摊成功，并更新数据库
    SalesReport ->> NSQ队列: 14.发布新主题拆解小票ResolveTicketTopic(ticketID)到消息队列
    
    Note over 客户端,NSQ队列: 下面开始执行小票拆解
    NSQ队列 ->> SalesReport: 15.接收到ResolveTicketTopic(ticketID)主题数据，处理函数:handleResolveTicket
    SalesReport ->> 数据库: 16.通过ticketID查询电子小票 数据并返回
    Note right of SalesReport: SalesTicketContent
    SalesReport ->> SalesReport: 17.验证电子小票si.ProcessStatus的状态是不是折扣分摊成功，
    loop 18.基础数据分解到四个基础表中
      SalesReport ->> 数据库 : 执行销售金额拆解方法：resolveSalesMoney
      Note right of SalesReport: SalesTicketAmount 销售金额统计
      
      SalesReport ->> 数据库 : 执行商品统计方法：resolveProductAmount
      Note right of SalesReport: SalesProductAmount  商品统计
      
      SalesReport ->> 数据库 : 执行支付统计方法：resolvePaymentAmount
      Note right of SalesReport: SalesPaymentAmount  支付统计
      
      SalesReport ->> 数据库 : 执行折扣统计方法：resolveDiscountAmount
      Note right of SalesReport: SalesDiscountAmount  折扣统计
		end
    SalesReport ->> 数据库: 19.折扣分摊通过后，将验证状态Status改为完成，并插入数据库SalesPaymentAmount
    Note right of SalesReport: SalesPaymentAmount
    SalesReport ->> NSQ队列: 20.发布新主题拆解小票ResolveTicketTopic(ticketID)到消息队列
    
```





### 3.拆分电子小票时序图：

```mermaid
sequenceDiagram
	Title: 电子小票拆分时序图
    participant 客户端
    participant SalesReport
    participant 数据库
    participant NSQ队列
    
    Note over 客户端,NSQ队列: 下面开始执行小票拆解
    NSQ队列 ->> SalesReport: 1.接收到ResolveTicketTopic(ticketID)主题数据，处理函数:handleResolveTicket
    SalesReport ->> 数据库: 2.通过ticketID查询电子小票 数据并返回
    Note right of SalesReport: SalesTicketContent
    SalesReport ->> SalesReport: 3.验证电子小票si.ProcessStatus的状态是不是折扣分摊成功，
    par 4.执行四个拆分流程
        SalesReport ->> SalesReport : 4.1销售金额统计：组装时间，统计商品数量和加料数量，生成SalesTicketAmount对象并存储到result切片中
    and
        SalesReport ->> SalesReport : 4.2商品统计信息：组装时间，遍历票中每一个商品，组装为一个{id：product}的map，遍历map组装为SalesProductAmount并添加到result切片中
    and
        SalesReport ->> SalesReport : 4.3支付统计信息：组装时间，遍历ticket.Payments，组装SalesPaymentAmount对象，存储到result切片中
    and
        SalesReport ->> SalesReport : 4.4折扣统计信息：组装时间，遍历ticket.Promotions，组装SalesDiscountAmount对象，存储到result切片中
    end
    SalesReport ->> 数据库: 5.开始插入数据InsertResolveTicket，通过EticketId查询model.SalesPaymentAmount，判断SalesPaymentAmount是否被重复拆解了。
    Note right of SalesReport: SalesPaymentAmount
    SalesReport ->> SalesReport	: 6.遍历model.Resolve数据，将每一个数据都存储到ResolveTicketPG中，
    
```





### 3.1处理主档缓存的topic作用

配合定时任务使用，定时任务里面发布的就是这个主题，这里接收到这个主题，就可以定时将订单里面的门店信息和商品信息存储到缓存中，以后查询就可以使用缓存里面的数据，不用再去主档拉取数据了，提高查询效率。

```go
	if err := DefaultMQ.RegistReceive(config.MetadataCacheTopic, handelMetadataCache); err != nil {
		log.Fatalf("Init mq receive for topic `%s` failed. Err: `%+v`", config.MetadataCacheTopic, err)
	}
```

```mermaid
sequenceDiagram
	Title: 电子小票拆分时序图
    participant SalesReport
    participant 数据库
    participant NSQ队列
    participant Grpc主档
    
    NSQ队列 -->> SalesReport: 1.获取定时任务的主题MetadataCacheTopic
		SalesReport ->> Grpc主档: 2.通过PartnerID，ScopeID，UserID查询门店信息
		Grpc主档 -->> SalesReport: 3.返回区域，门店信息
    SalesReport ->> 数据库: 4.将门店信息存储到数据库中
    Note over SalesReport,数据库: store_caches
```









### 4.查询sales报表

接口：GET: /api/v1//tickets

返回：{"rows":result,"total",total}

```mermaid
sequenceDiagram
	Title: 查询sales时序图
    participant 客户端
    participant SalesReport
    participant 数据库
    
    客户端 ->> SalesReport: 1.发送GET请求，带上store_id，limit，offset等字段，
    SalesReport ->> 数据库: 2.通过storeId执行QuerySalesInfo，查询销售信息，并返回
    Note right of SalesReport: SalesTicketContent
    SalesReport ->> 数据库: 3.校验门店id是否在权限内
    SalesReport ->> SalesReport: 4.校验门店id是否在权限内，并检查日期是否合格，SearchSales
    SalesReport ->> 数据库: 5.通过store_id查询销售，遍历结果result，组装storeIds和oauthIds
    SalesReport ->> 数据库: 6.通过storeIds获取GetIdsFieldMap，通过oauthIds获取authorFieldMap
    SalesReport ->> SalesReport: 7.组装StoreName和UpdatedName字段，形成一个result结果
    SalesReport -->> 客户端: 8.返回销售信息{"rows":result,"total",total}
    
```



### 5.更新sales数据

接口：PUT: /api/v1//tickets

返回：{"err": ""}

```mermaid
sequenceDiagram
	Title: 更新sales时序图
    participant 客户端
    participant SalesReport
    participant 数据库
    participant NSQ队列
    
    客户端 ->> SalesReport: 1.发送PUT请求，带上id和content_modified等字段
    SalesReport ->> 数据库: 2.将content_modified字段转化为string，更新数据库
    Note right of SalesReport: SalesTicketContent
    SalesReport ->> NSQ队列: 3.更新成功后，发布WaitCheckTopic到消息队列中
    Note over 客户端,NSQ队列: 下面开始执行handleWaitCheckTicket 
    Note over 客户端,NSQ队列: 处理检查小票..............
    
    
```



### 6.回滚sales数据

接口：PUT: /api/v1//tickets/rollback

返回：{"err": ""}

```mermaid
sequenceDiagram
	Title: 更新sales时序图
    participant 客户端
    participant SalesReport
    participant 数据库
    
    客户端 ->> SalesReport: 1.发送PUT请求，带上ticket_id,
    SalesReport ->>数据库: 2.通过ticket_id查询到该小票内容
    SalesReport ->> SalesReport: 3.判断小票status是不是新增，
    SalesReport ->>数据库: 4.更新小票的updated字段内容
    SalesReport -->>客户端: 5.返回err
```





### 7.门店交易报表:StoreSales

POST：/api/v1/report/sales/store

Return：{"rows":result,"total",total}

```mermaid
sequenceDiagram
	Title: 门店sales
    participant 客户端
    participant SalesReport
    participant 数据库
    
    客户端 ->> SalesReport: 1.发送POST请求，带上CommonRequest中各个字段
    SalesReport ->> SalesReport: 2.解析CommonRequest中各个字段，执行QueryStoreSales
    SalesReport ->> SalesReport: 3.将SearchIds和CategoryIds和ProductIds等组装为一个ids切片
    SalesReport ->> SalesReport: 4.组装RepoCondition并过滤掉不认识的字段
    SalesReport ->> SalesReport: 5.通过trackId, condition组装sql语句：rowSQL, summarySQL 
    SalesReport ->> 数据库: 6.使用gorouting查询到rows，summary
    Note right of SalesReport: 根据condition.IsPre判断store_sales/sales_ticket_amounts(是否使用预计算)
    SalesReport ->> SalesReport: 7.组装packagingForStoreSales报表信息，
    SalesReport -->> 客户端:8.返回{Rows: rows, Summary: summary}
    
```



### 8.单品交易报表:ProductSales

POST：/api/v1/report/sales/product

Return：{"rows":result,"total",total}

```mermaid
sequenceDiagram
	Title: 商品sales
    participant 客户端
    participant SalesReport
    participant 数据库
    
    客户端 ->> SalesReport: 1.发送POST请求，带上CommonRequest中各个字段
    SalesReport ->> SalesReport: 2.解析CommonRequest中各个字段，执行QueryProductSales
    SalesReport ->> SalesReport: 3.组装查询条件condition，根据trackId, condition生成rowSQL, summarySQL
    SalesReport ->> 数据库: 4.使用rowSQL, summarySQL查询结果{Rows: rows, Summary: summary}
    Note right of SalesReport: 根据condition.IsPre判断store_sales/sales_ticket_amounts(是否使用预计算)
    SalesReport -->> 客户端: 5.返回
    
```

### 9.类别交易报表:CategorySales

POST：/api/v1/report/sales/category

Return：{"rows":result,"total",total}

```mermaid
sequenceDiagram
	Title: 商品sales
    participant 客户端
    participant SalesReport
    participant 数据库
    
    客户端 ->> SalesReport: 1.发送POST请求，带上CommonRequest中各个字段
    SalesReport ->> SalesReport: 2.解析CommonRequest中各个字段，执行QueryCategorySales
    SalesReport ->> SalesReport: 3.组装查询条件condition，根据trackId, condition生成rowSQL, summarySQL
    SalesReport ->> 数据库: 4.使用rowSQL, summarySQL查询结果{Rows: rows, Summary: summary}(是否使用预计算)
    Note right of SalesReport: 根据condition.IsPre判断store_sales/sales_ticket_amounts
    SalesReport -->> 客户端: 5.返回
```

### 10.支付统计报表:PaymentStatistics

POST：/api/v1/report/sales/category

Return：{"rows":result,"total",total}

```mermaid
sequenceDiagram
	Title: 商品sales
    participant 客户端
    participant SalesReport
    participant 数据库
    
    客户端 ->> SalesReport: 1.发送POST请求，带上CommonRequest中各个字段
    SalesReport ->> SalesReport: 2.解析CommonRequest中各个字段，执行PaymentStatistics
    SalesReport ->> SalesReport: 3.组装查询条件condition，根据trackId, condition生成rowSQL, summarySQL
    SalesReport ->> 数据库: 4.使用rowSQL, summarySQL查询结果{Rows: rows, Summary: summary}(是否使用预计算)
    Note right of SalesReport: 根据condition.IsPre判断store_sales/sales_ticket_amounts
    SalesReport -->> 客户端: 5.返回
    
```
