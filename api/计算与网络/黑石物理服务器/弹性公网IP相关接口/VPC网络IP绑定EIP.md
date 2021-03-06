## 功能描述
EipBmBindVpcIp接口用于绑定黑石弹性公网IP到黑石VPC的IP上（非黑石物理机IP）。区别于[EipBmBindRs](/document/product/386/6673)，该接口主要适用于客户使用黑石的半托管服务或者在黑石物理集群中创建自己的云服务器，同时又需要这些半托管的设备或者云服务器出公网的场景。

接口访问域名: bmeip.api.qcloud.com


## 请求

### 请求示例

```
GET https://bmeip.api.qcloud.com/v2/index.php?
	Action=EipBmBindVpcIp
	&<公共请求参数>
	&eipId=<EIP实例ID>
	&unVpcId=<vpc的ID>
	&vpcIp=<内网IP>
```

### 请求参数

以下请求参数列表仅列出了接口请求参数，正式调用时需要加上公共请求参数，见[公共请求参数页面](/document/product/386/6718)。其中，此接口的Action字段为 EipBmBindVpcIp。

| 参数名称 | 必选 | 类型 | 描述 |
|---------|---------|---------|---------|
| eipId | 是 | String | EIP实例ID，格式形如：eip-testid |
| unVpcId | 是 | String | EIP归属的VPC的标识，格式形如：vpc-k7j1t2x1，可通过[查询私有网络列表](/document/product/386/6646)返回的字段unVpcId获得 |
| vpcIp | 是 | String | VPC内IP，此IP地址必须通过[申请内网IP接口](/document/product/386/7337)申请获得或者通过[注册子网IP](/document/product/386/7925)接口注册，否则无法绑定EIP|

## 响应

### 响应示例

```
{
 "code": 0,
 "message": "",
 "codeDesc": "Success",
 "data": {
  "requestId": "<EIP异步任务ID>"
 }
}
```
### 响应参数

响应参数部分包含两层结构，外层展示接口的响应结果，内层展示具体的接口内容（此接口中为异步任务ID）

| 参数名称 | 类型 | 描述 |
|---------|---------|---------|
| code |  Int | 错误码, 0: 成功, 其他值: 失败，具体含义可以参考[错误码](/document/product/386/6725)。 |
| message |   String | 错误信息 |
| data |   Array | 返回异步任务信息 |

Data结构

|参数名称|类型|描述|
|---|---|---|
| data.requestId | Int | EIP绑定VPC网络IP异步任务ID，可以通过[查询EIP任务状态](/document/product/386/6670)查询任务状态|

## 错误码

|错误代码|英文提示|错误描述|
|---|---|---|
|9003|ParamInvalid|请求参数不正确|
|9006|InternalErr|内部数据操作异常|
|30009|EipNotExist|操作的EIP记录不存在|
|30010|EipStateCannotOp|EIP当前状态不允许绑定|
|30012|EipInArrears|操作的EIP已欠费|
|-50000|VpcIdInvalid|VpcId参数不正确|
|-49999|VpcIpBinded|操作的VpcIp已经绑定EIP|
|-49998|VpcIpNotExist|操作的VpcIp不存在|
|-49997|VpcIpInvalid|操作的VpcIp属于物理机IP|
|-49996|VpcIpSubnetInvalid|操作的VpcIp的子网信息异常|
|-49995|TunnelEipNotSuport|隧道模式的EIP暂不支持绑定VpcIP|
|-49994|VpcIpNotApplyed|操作的VpcIp未申请|

## 实际案例

### 输入

```
GET https://bmeip.api.qcloud.com/v2/index.php?
	Action=EipBmBindVpcIp
	&SecretId=AKIDlfdHxN0ntSVt4KPH0xXWnGl21UUFNoO5
	&Nonce=61431
	&Timestamp=1507728683
	&Region=bj
	&eipId=eip-kpge33wo
	&unVpcId=vpc-k7j1t2x1
	&vpcIp=10.1.1.2
	&Signature=ohpWkOBXHyE11XgK2KIH%2F14bMrc%3D
```

### 输出

```
{
    "code": 0,
    "message": "",
    "codeDesc": "Success",
    "data": {
        "requestId": 100000
    }
}
```

