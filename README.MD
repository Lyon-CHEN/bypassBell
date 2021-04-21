# 旁路Bell的路由器，换上其它高性能路由器

加拿很大概率会用上Bell的光纤网络。在开通服务时都会统一配置HH 3000路由器，但因为各种原因又需要使用自己的路由器，有以下好处：
	• 换运营商，家里的设备不用改wifi配置
	• 提高Wi-Fi性能
	• 不受运营商的设备限制
	• 避免多层路由NAT地址转换
	
***	
## 一，起因/背景
因为黑五买了一个群晖(Synology) DS920+ NAS放家里，接着就想随时可以通过VPN连接回家里的Synology NAS服务器。自己的主力手机是Iphone，所以VPN协议也就决定使用IOS原生支持L2TP/IPSEC。
在Synology配置VPN大概步骤如下，
	-1，群晖NAS申请打开DDNS，配置自己的域名。如果有静态互联网IP地址，直接使用IP也可以。
	-2，群晖NAS开启VPN L2TP服务
![image](https://github.com/Lyon-CHEN/bypassBell/blob/main/images/1.png)

	-3，由器上配置了L2TP使用的UDP 1701/500/4500端口的转发到NAS
![image](https://github.com/Lyon-CHEN/bypassBell/blob/main/images/2.png)

测试后发现通过DDNS域名VPN连不上，但在局域网内通过IP地址VPN可以正常连接。定位问题应该是路由器在上，此时网络拓补如下：
![image](https://github.com/Lyon-CHEN/bypassBell/blob/main/images/3.png)
注意在Bell HH3000上已经将XiaoMi 路由器配置成DMZ, 但在Xiaomi Router上抓包收不到任何L2TP的UDP包。
Google后才知道Bell在路由器底层屏蔽了一些协议和端口, 配置DMZ也没用。而L2TP的UDP端口也是固定的能不变的，所以如果想用VPN连回家里，只能想办法把Bell 的HH3000给替换掉了。
![image](https://github.com/Lyon-CHEN/bypassBell/blob/main/images/4.png)

***	

## 二，方案
Bell路由器使用SFP接口的光纤，同时使用了VLAN，这里有如下两个方案，方案A配置简单但需要路由器支持VLAN（PVID) 功能，方案B可以更灵活

### A，需要路由器支持VLAN功能，使用SFP光电转换器，
	TP-Link MC220L (From <https://www.amazon.ca/gp/product/B003CFATL0/> 
	步骤如下：
		-1，MC220L光口连Bell 光纤，电口连路由器WAN，类似于网桥
		-2,    路由器WAN口PVID配置成35  (默认VLAN）
		-3a，(Bell)配置WAN口上网为PPPOE拨号，用户名和密码在Bell Account里可以查（没有测试过，只在网上看到的示例）
		-3b,  (aliant.bell altiantic地区) 配置WAN口上网设置为DHCP
![image](https://github.com/Lyon-CHEN/bypassBell/blob/main/images/5.png)

	其它配置不变，能分配到IP就成功。需要你的路由器支持VLAN PVID功能，比如ASUS或者Openwrt
	
### B，使用支持VLAN和SFP接口的交换机，比如 
	NETGEAR GC110 <https://www.amazon.ca/gp/product/B01NBBA1HQ/>  
	步骤如下：
		-1，GC110 光口(g10)连Bell 光纤，电口g8连路由器WAN
		-2，新增VLAN 35, 并将g8和g10加入VLAN35
![image](https://github.com/Lyon-CHEN/bypassBell/blob/main/images/6.png)
		-3, g8配置成access mode,Access VLAN ID为35，Native VLAN ID为35
![image](https://github.com/Lyon-CHEN/bypassBell/blob/main/images/7.png)

		- 4a，(bell)配置WAN口上网为PPPOE拨号，用户名和密码在Bell Account里可以查（没有测试过，只在网上看到的示例）
		- 4b,  (aliant.bell altiantic地区) 配置WAN口为DHCP
![image](https://github.com/Lyon-CHEN/bypassBell/blob/main/images/8.png)


 


	


