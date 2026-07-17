# 系统架构图模板

使用此模板快速创建系统架构设计文档。

---

## 基础三层架构模板

### 使用场景
传统的三层架构系统（表现层、业务逻辑层、数据层）。

### 架构图

```mermaid
graph TB
    subgraph Client["客户端"]
        WEB[Web 浏览器]
        MOBILE[移动应用]
    end
    
    subgraph Presentation["表现层"]
        API[REST API]
        VIEW[视图层]
    end
    
    subgraph Business["业务逻辑层"]
        AUTH[认证服务]
        USER[用户服务]
        ORDER[订单服务]
        PAYMENT[支付服务]
    end
    
    subgraph Data["数据层"]
        USERDB[(用户数据库)]
        ORDERDB[(订单数据库)]
        CACHE[(缓存)]
    end
    
    subgraph External["外部服务"]
        PAYAPI[第三方支付API]
        EMAILAPI[邮件服务]
    end
    
    WEB --> API
    MOBILE --> API
    API --> AUTH
    AUTH --> CACHE
    API --> USER
    API --> ORDER
    API --> PAYMENT
    USER --> USERDB
    USER --> CACHE
    ORDER --> ORDERDB
    PAYMENT --> PAYAPI
    PAYMENT --> EMAILAPI
```

### 层级说明

- **客户端**: 使用系统的应用程序
- **表现层**: 提供 API 和用户界面
- **业务逻辑层**: 核心业务处理
- **数据层**: 数据存储和缓存
- **外部服务**: 第三方集成

---

## 微服务架构模板

### 使用场景
使用微服务架构的分布式系统。

### 架构图

```mermaid
graph TB
    subgraph Client["客户端"]
        WEB[Web]
        MOBILE[Mobile]
        ADMIN[Admin]
    end
    
    subgraph Gateway["网关层"]
        LB[负载均衡器]
        APIGW[API 网关]
        AUTH_GW[认证网关]
    end
    
    subgraph Services["微服务"]
        USER_SVC[👤 用户服务]
        PRODUCT_SVC[📦 产品服务]
        ORDER_SVC[📋 订单服务]
        PAYMENT_SVC[💳 支付服务]
        DELIVERY_SVC[🚚 配送服务]
    end
    
    subgraph Cache["缓存层"]
        REDIS[Redis 缓存]
    end
    
    subgraph Database["数据层"]
        USERDB[(用户DB)]
        PRODUCTDB[(产品DB)]
        ORDERDB[(订单DB)]
    end
    
    subgraph MessageQueue["消息队列"]
        MQ[RabbitMQ/Kafka]
    end
    
    subgraph Monitor["监控和日志"]
        LOG[日志系统]
        MONITOR[监控系统]
    end
    
    WEB --> LB
    MOBILE --> LB
    ADMIN --> LB
    LB --> APIGW
    APIGW --> AUTH_GW
    AUTH_GW --> USER_SVC
    AUTH_GW --> PRODUCT_SVC
    AUTH_GW --> ORDER_SVC
    AUTH_GW --> PAYMENT_SVC
    AUTH_GW --> DELIVERY_SVC
    
    USER_SVC --> USERDB
    PRODUCT_SVC --> PRODUCTDB
    ORDER_SVC --> ORDERDB
    
    USER_SVC --> REDIS
    PRODUCT_SVC --> REDIS
    ORDER_SVC --> REDIS
    
    ORDER_SVC --> MQ
    PAYMENT_SVC --> MQ
    DELIVERY_SVC --> MQ
    
    USER_SVC --> LOG
    PRODUCT_SVC --> LOG
    ORDER_SVC --> LOG
    PAYMENT_SVC --> LOG
    
    USER_SVC --> MONITOR
    PRODUCT_SVC --> MONITOR
    ORDER_SVC --> MONITOR
    PAYMENT_SVC --> MONITOR
```

### 组件说明

- **网关层**: 请求路由和负载均衡
- **微服务**: 独立的业务服务
- **缓存层**: 提高性能
- **数据层**: 分散的数据存储
- **消息队列**: 服务间异步通信
- **监控**: 系统可观测性

---

## 高可用架构模板

### 使用场景
支持高可用、容灾备份的企业级系统。

### 架构图

```mermaid
graph TB
    subgraph Region1["主机房"]
        LB1[负载均衡器]
        APP1_1[应用服务器1]
        APP1_2[应用服务器2]
        APP1_3[应用服务器3]
        DB1[(主数据库)]
        CACHE1[(缓存1)]
    end
    
    subgraph Region2["备用机房"]
        LB2[负载均衡器]
        APP2_1[应用服务器1]
        APP2_2[应用服务器2]
        DB2[(从数据库)]
        CACHE2[(缓存2)]
    end
    
    subgraph CDN["CDN 层"]
        CDN_GW[CDN 网关]
    end
    
    subgraph Storage["存储"]
        S3[对象存储]
        EBS[块存储]
    end
    
    User[用户] --> CDN_GW
    CDN_GW --> LB1
    CDN_GW --> LB2
    
    LB1 --> APP1_1
    LB1 --> APP1_2
    LB1 --> APP1_3
    
    LB2 --> APP2_1
    LB2 --> APP2_2
    
    APP1_1 --> DB1
    APP1_2 --> DB1
    APP1_3 --> DB1
    
    APP1_1 --> CACHE1
    APP1_2 --> CACHE1
    
    APP2_1 --> DB2
    APP2_2 --> DB2
    APP2_1 --> CACHE2
    APP2_2 --> CACHE2
    
    DB1 -.复制.-> DB2
    CACHE1 -.同步.-> CACHE2
    
    APP1_1 --> S3
    APP1_1 --> EBS
```

### 特点

- **多区域部署**: 主备机房架构
- **负载均衡**: 流量分散
- **数据复制**: 主从同步
- **缓存同步**: 保证数据一致性
- **CDN**: 加速静态内容

---

## 云原生架构模板

### 使用场景
使用 Kubernetes 等容器编排的云原生应用。

### 架构图

```mermaid
graph TB
    subgraph Kubernetes["Kubernetes 集群"]
        subgraph NS1["命名空间: Default"]
            POD1[Pod - 用户服务]
            POD2[Pod - 订单服务]
            POD3[Pod - 支付服务]
        end
        
        subgraph NS2["命名空间: Database"]
            STATEFULSET[StatefulSet - 数据库]
        end
        
        subgraph Storage["存储"]
            PVC1[PVC]
            PVC2[PVC]
        end
        
        subgraph Config["配置管理"]
            CM[ConfigMap]
            SECRET[Secret]
        end
        
        subgraph Ingress["入口"]
            IG[Ingress Controller]
        end
    end
    
    subgraph External["外部服务"]
        REGISTRY[容器镜像仓库]
        MONITOR[Prometheus]
        LOG[ELK 日志]
    end
    
    User[用户] --> IG
    IG --> POD1
    IG --> POD2
    IG --> POD3
    
    POD1 --> STATEFULSET
    POD2 --> STATEFULSET
    POD3 --> STATEFULSET
    
    STATEFULSET --> PVC1
    STATEFULSET --> PVC2
    
    POD1 --> CM
    POD1 --> SECRET
    
    REGISTRY -.镜像来源.-> POD1
    REGISTRY -.镜像来源.-> POD2
    
    POD1 -.监控.-> MONITOR
    POD1 -.日志.-> LOG
```

---

## 创建自己的架构图

### 步骤 1: 确定层级

```mermaid
graph TB
    subgraph Layer1["表现层"]
        A1[组件1]
    end
    subgraph Layer2["业务层"]
        B1[组件2]
    end
    subgraph Layer3["数据层"]
        C1[组件3]
    end
```

### 步骤 2: 添加具体组件

```mermaid
graph TB
    A1[API Gateway] --> B1[用户服务]
    B1 --> C1[(数据库)]
```

### 步骤 3: 添加外部服务

```mermaid
graph TB
    A1[客户端] --> A2[API]
    A2 --> B1[服务1]
    B1 --> C1[(数据库)]
    B1 --> D1[第三方API]
```

### 步骤 4: 添加 subgraph 分组

```mermaid
graph TB
    subgraph Clients["客户端"]
        A1[Web]
        A2[Mobile]
    end
    
    subgraph Backend["后端服务"]
        B1[API]
        B2[Service]
    end
```

---

## 最佳实践

### ✅ 推荐做法

- 使用清晰的层级划分
- 组件使用有意义的名称
- 标注数据流向
- 使用 subgraph 进行逻辑分组
- 包含所有关键组件
- 添加文字说明

### ❌ 避免

- 过于复杂，单个图表包含过多内容
- 模糊的组件名称
- 遗漏关键组件
- 不清楚的数据流
- 混乱的连接线

---

## 导出和使用

### 在 GitHub 中

直接保存此文件，GitHub 会自动渲染架构图。

### 导出为图片

```bash
mmdc -i architecture.md -o architecture.png
```

### 在演示中使用

```bash
# 导出为 SVG 便于缩放
mmdc -i architecture.md -o architecture.svg
```

---

## 高级技巧

### 1. 添加图例

```mermaid
graph TB
    A[矩形]
    B(圆角)
    C{菱形}
    D([椭圆])
    E[[子程序]]
    F[(圆柱)]
```

### 2. 连接线样式

```mermaid
graph LR
    A -->|实线| B
    A -.->|虚线| C
    A ==>|粗线| D
```

### 3. 添加注释

```mermaid
graph TB
    A[节点]
    B[节点]
    A --> B
    N["注: 这是一个注释"]
```

---

## 相关文档

- [Diagram Drawing Ability](../abilities/diagram-drawing.md)
- [Mermaid 图表示例](../examples/mermaid-examples.md)
- [最佳实践指南](../abilities/diagram-drawing.md#最佳实践)

---

## 下一步

1. 复制此模板并修改为您的系统
2. 在项目文档中使用
3. 与团队分享和讨论
4. 定期更新以反映系统最新设计
