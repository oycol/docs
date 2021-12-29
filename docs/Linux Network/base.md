**若Linux上到某主机有多条路由可以选择，这时候会挑选优先级高的路由。在Linux中，路由条目的优先级确定方式是先匹配掩码位长度，再比较管理距离(比如metric)**,若路由条目的掩码长度相同，则比较节点之间的管理距离，管理距离短的生效。

U (route is up)

H (target is a host)

G (use gateway，也即是设置了下一跳的路由条目)

```
route [add/del] [-host/-net/default] [address[/mask]] [netmask] [gw] [dev]

选项说明：
add/del：增加或删除路由条目
-net：增加或删除的是一条网络路由
-host：增加或删除的是一条主机路由
default：增加或删除的是一条默认路由
netmask：明确使用netmask关键字指定掩码，要可以不使用该选项直接在地址上使用cidr格式的掩码，即IP/MASK。
gw：指定下一跳的地址。要求下一跳地址必须是能到达的，且一般是和本网段直连的接口。
dev：强制将路由条目关联到指定的接口上。一般内核会自动判断路由条目应该关联到哪个网络接口。
```