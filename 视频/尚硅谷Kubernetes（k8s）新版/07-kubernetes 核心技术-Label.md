# 七、kubernetes 核心技术-Label

## 1 Label 概述

Label 是 Kubernetes 系统中另一个核心概念。一个 Label 是一个 key=value 的键值对，其中 key 与 value 由用户自己指 定。Label 可以附加到各种资源对象上，如 Node、Pod、Service、RC，一个资源对象可以定义任意数量 Label，同一个 Label 也可以被添加到任意数量的资源对象上，Label 通常在资源对象定义时确定，也可以在对象创建后动态 添加或删除。

Label 的最常见的用法是使用 metadata.labels 字段，来为对象添加 Label，通过 spec.selector 来引用对象

## 2 Label 示例

```yaml
apiVersion: v1
kind: ReplicationController
metadata:
  name: nginx
spec:
  replicas: 3
  selector:
    app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80

---

apiVersion: v1
kind: Service
metadata:
  name: nginx
spec:
  type: NodePort
  ports:
  - port: 80
    nodePort: 32333
  selector:
    app: nginx
```

Label 附加到 Kubernetes 集群中各种资源对象上，目的就是对这些资源对象进行分组管理，而分组管理的核心就 是 Label Selector。Label 与 Label Selector 都是不能单独定义，必须附加在一些资源对象的定义文件上，一般附加 在 RC 和 Service 的资源定义文件中

