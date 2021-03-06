## 简介
弹性公网 IP（EIP），是与账号关联的静态 IP 地址。弹性公网 IP 未进行释放前， 您可以将其一直保留于您的账号中。 相较于公网 IP 仅可跟随云服务器一起申请释放，弹性公网 IP 可以与云服务器的生命周期解耦，作为云资源单独进行操作。

例如，若您需要保留某个与业务强相关的公网IP，可以将其转为弹性公网IP保留在您的账号中。

## 规则与限制
### 使用规则
- 弹性公网 IP 地址同时适用于基础网络和私有网络的实例，以及私有网络中的 [NAT 网关](https://cloud.tencent.com/document/product/215/20079) 实例。
- 弹性 IP 地址与 CVM 实例绑定时，实例的当前公网 IP 地址会被释放。
- 销毁 CVM/NAT 网关实例，会断开与弹性 IP 地址的关联。
- 弹性公网 IP 计费规则请参考 [弹性公网 IP 计费](https://cloud.tencent.com/document/product/213/17156)。
- 弹性公网 IP 操作步骤请参考 [弹性公网 IP](https://cloud.tencent.com/document/product/213/16586)。
 
### 使用限制

| 资源 | 限制 |
|---------|:---------:|
| 每个腾讯云账户每个地域（Region）配额弹性公网 IP 个数 | 20个 |
| 每个腾讯云账户各个地域每天申购次数	 | 配额数 \* 2 次 |
| 解绑 EIP 时，每个账户每天可免费重新分配公网 IP 的次数 | 10次 |

## 弹性公网 IP 不通原因排查方法
弹性公网 IP 不通一般有如下原因：
- 弹性公网 IP 没有绑定到云资源上，具体绑定方法请参见 [弹性公网 IP 绑定云产品](https://cloud.tencent.com/document/product/213/16586#.E5.BC.B9.E6.80.A7.E5.85.AC.E7.BD.91-ip-.E7.BB.91.E5.AE.9A.E4.BA.91.E4.BA.A7.E5.93.81)。
- 查看 CVM 实例内部是否有安全策略，如果 CVM 实例有安全组策略，例如：禁止8080端口访问，那么弹性公网 IP 的8080端口也是无法访问的。

## NAT 网关和弹性公网 IP 的使用
NAT 网关和弹性公网 IP 是云服务器访问 Internet 的两种方式，您可以选择其中一种或两种用于您的公网访问架构设计：
### 方案1：只使用 NAT 网关
**云服务器不绑定弹性公网 IP，所有访问 Internet 流量通过 NAT 网关转发。**
此种方案中：
云服务器访问 Internet 的流量会通过内网转发至 NAT 网关，不受云服务器购买时的公网带宽上限限制，该部分流量属于内网互通（无费用产生），NAT 网络费用是独立计算，即：云服务器通过 NAT 网关访问公网的费用扣费在 NAT 网关实例上。

### 方案2：只使用弹性公网 IP
**云服务器只绑定弹性公网 IP，不使用 NAT 网关。**
此种方案中：
云服务器所有访问 Internet 的流量都通过弹性公网 IP 流出，受到云服务器购买时的公网带宽上限限制。访问公网产生的相关费用，根据云服务器网络计费模式而定，详情请参见 [弹性公网 IP 计费](https://cloud.tencent.com/document/product/213/17156)。

### 方案3：同时使用 NAT 网关和弹性公网 IP
**云服务器绑定了弹性公网 IP，同时所在子网路由访问 Internet 流量指向了 NAT 网关。**
此种方案中：
- 所有云服务器主动访问 Internet 的流量**只通过内网转发至 NAT 网关**，回包也经过 NAT 网关返回至云服务器，此部分流量不受云服务器购买时的公网带宽上限限制，NAT 网关产生的网络流量费用不会占用云服务器的公网带宽出口。
- 如果来自 Internet 的流量主动访问云服务器的弹性公网 IP，则云服务器回包统一通过弹性公网 IP 返回，这样产生的公网出流量，受到云服务器购买时的公网带宽上限限制。访问公网产生的相关费用，根据云服务器网络计费模式而定，详情请参见 [弹性公网 IP 计费](https://cloud.tencent.com/document/product/213/17156)。

>!如果您的账号开通了带宽包共享带宽功能，则 NAT 网关产生的出流量按照带宽包整体结算（不再重复收取0.8元/GB 的网络流量费）。建议您限制 NAT 网关的出带宽，避免因 NAT 网关出带宽过高产生高额的带宽包费用。

## 操作指南
控制台操作详情，请参见 [操作总览](https://cloud.tencent.com/document/product/215/20148)。
