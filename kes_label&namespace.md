###第一部分 pod

1. 什么是node？

2. 什么是pod？pod是逻辑上的主机，和物理主机没什么区别。 代表了Kubemetes中的基本构建模块。部署的时候针对一组pod进行部署和操作

3. pod和node是什么关系？

4. pod和docker container是什么关系？

   多个Container可以运行在同一个pod上，共享同一个端口和IP地址。



当前集群有几个node？1个

几个pod？1个



### 第二部分 label

1. 什么是label？标签是可以关联到任意资源的键值对。只要在这个资源内的所有标签的key值唯一，就可以为一个资源关联多个标签。

2. label的作用：对资源进行标记，从而无需新建资源。

3. #### 增

   1. yaml文件

      pod-with-label.yaml

      ```yaml
      apiVersion: v1
      kind: Pod
      metadata:
        name: pod-with-label
        labels:
          hello-label: world
      spec:
        containers:
        - image: luksa/kubia
          name: kubia
          ports:
          - containerPort: 8080
          	protocol: TCP
      ```

      then

      `kubectl create -f  pod-with-label.yaml`

      2. kubectl label node pod-with-label hello-label=world

      #### 改

      `kubectl label pod-with-label hello-label=universe --overwrite  `

      #### 查询

      * 含有标签"hi-label"的所有pod `kubectl get po -l hi-label`
      * 不含有标签"hi-label"的所有pod `kubectl get po -l !hi-label`
      * 含有标签"hi-lable"且值为"universe"的所有pod `kubectl get po -l !hi-labe=universe`
      * 含有标签"hi-lable"且值不为"universe"的所有pod `kubectl get po -l hi-label!=universe`
      * 请使用command删除含有标签"hi-label"的所有pod `kubectl delete pods -l hi-label`

### 第三部分

1. 什么是namespace？ 方便将对象分割成完全独立不重叠的组，Kubernates简单的为命名空间提供了一个作用域

2. ####查

   2.1 查询所有的namespace： `kubectl get ns`

   查询universe 里的所有pod： `kubectl get po --namespace universe`

   #### 增

   请分别通过yaml文件和command，创建一个名为world的namespace

   `world.yaml`

   ```yaml
   apiVersion: v1
   kind: Namespace
   metadata:
     name: world
   ```

   `kuectl create -f world.yaml`

或者

`kubectl create namespace world`

​	请分别通过yaml文件和command，创建一个pod使其隶属于上述创建的namespace

`kubectl create -f node.yaml -n custom-namespace`

#### 	删

​	`kubectl delete po --all --namespace=universe`

