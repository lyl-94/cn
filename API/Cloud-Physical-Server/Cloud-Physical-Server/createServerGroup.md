# createServerGroup


## 描述
创建虚拟服务器组

## 请求方式
PUT

## 请求地址
https://cps.jdcloud-api.com/v1/regions/{regionId}/serverGroups

|名称|类型|是否必需|默认值|描述|
|---|---|---|---|---|
|**regionId**|String|True| |地域ID，可调用接口（describeRegiones）获取云物理服务器支持的地域|

## 请求参数
|名称|类型|是否必需|默认值|描述|
|---|---|---|---|---|
|**clientToken**|String|False| |由客户端生成，用于保证请求的幂等性，长度不能超过36个字符；<br/><br>如果多个请求使用了相同的clientToken，只会执行第一个请求，之后的请求直接返回第一个请求的结果<br/><br>|
|**serverGroupSpec**|ServerGroupSpec|True| |虚拟服务器组配置|

### ServerGroupSpec
|名称|类型|是否必需|默认值|描述|
|---|---|---|---|---|
|**loadBalancerId**|String|False| |负载均衡ID|
|**name**|String|False| |虚拟服务器组名称|

## 返回参数
|名称|类型|描述|
|---|---|---|
|**result**|Result| |
|**requestId**|String| |

### Result
|名称|类型|描述|
|---|---|---|
|**serverGroupId**|String|服务器组ID|

## 返回码
|返回码|描述|
|---|---|
|**200**|OK|
|**400**|Bad request|
|**404**|Not found|
|**500**|Internal server error|
|**503**|Service unavailable|
