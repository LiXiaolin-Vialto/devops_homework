**什么是存活探针？（liveness probe）**

Kubemetes 可以通过存活探针 (liveness probe) 检查容器是否还在运行。 可以 为 pod 中的每个容器单独指定存活探针。 如果探测失败， Kubemetes 将定期执行探 针并重新启动容器。



**存活探针有那三种机制？**

1. HTTPGET 探针对容器的 IP 地址（你指定的端口和路径）执行 HTTP GET 请求。
2. TCP套接字探针尝试与容器指定端口建立TCP连接。
3. Exec 探针在容器内执行任意命令，并检查命令的退出状态码。

**创建一个.net core api 项目， 该项目有个Get Api 路由时 /health， 访问到一定次数，（比如10次）的时候，返回400错误, 将该项目发布到docker hub**



**基于该项目创建一个pod， 并且创建一个Http存活探针，探针指向 /health 接口， 该探针需要在容器启动后等待30秒再工作**

`查看pod重启原因`，

**什么是ReplicationController**

ReplicationController是 一 种Kubemetes资源，可确保它的pod始终保持运行状态。 如果pod因任何原因消失（例如节点从集群中消失或由于该pod已从节点中逐出）， 则ReplicationController 会注意到缺少了pod并创建替代p od。



**ReplicationController 三部分都是什么？**

> label selector ( 标签选择器）， 用于确定ReplicationController作用域中有哪些 pod

> replica count (副本个数）， 指定应运行的pod 数量

> pod template (pod模板）， 用于创建新的pod 副本

创建一个ReplicationController, pod为上面的api项目，数量为3个

删除其中一个pod后，还有几个存在？

4，其中一个已经终止，controller又新建了一个pod

RelicationController中的某个Pod移出/移入其作用域

Pod移出ReplicationController的作用域后，其Pod数量是几个？

3，controlelr又创建了一个pod

将Pod移入ReplicationController之后，其Pod数量是几个？

3

存活探针与ReplicationController有什么区别？

Pods被删除后，存活探针是否还起作用