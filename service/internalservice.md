{{indexmenu_n>10}}
## 通过内网ULB访问Service

> 注意：除外网EIP外，ULB相关参数目前均不支持Update，如不确认如何填写，请咨询UCloud 技术支持。


仅需要通过 metadata.annotations 指定 load-balancer-type为inner，其他参数都有默认值，可不填写。

暂不支持UDP类型的Service。

```
apiVersion: v1
kind: Service
metadata:
  name: ucloud-nginx-out-tcp-new
  labels:
    app: ucloud-nginx-out-tcp-new
  annotations:
    "service.beta.kubernetes.io/ucloud-load-balancer-type": "Inner"  
     # ULB类型，默认为Outer，支持Outer、Inner
    "service.beta.kubernetes.io/ucloud-load-balancer-vserver-protocol": "TCP"       
     # 用于声明ULB7还是ULB4，TCP与UDP等价，均表示为ULB4，HTTP和HTTPS等价，均标示为ULB7。
spec:
  type: LoadBalancer
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  selector:
    app: ucloud-nginx-out-tcp-new
---
apiVersion: v1
kind: Pod
metadata:
  name: test-nginx-out-tcp
  labels:
    app: ucloud-nginx-out-tcp-new
spec:
  containers:
  - name: nginx
    image: uhub.service.ucloud.cn/ucloud/nginx:1.9.2
    ports:
    - containerPort: 80
```