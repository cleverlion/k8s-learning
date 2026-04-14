# apiVersion: API 版本，PV 属于核心资源组 v1
apiVersion: v1
# kind: 资源类型，这里是 PersistentVolume（持久卷）
kind: PersistentVolume
metadata:
  # name: PV 的名称，在集群中唯一，用于 PVC 绑定或管理员识别
  name: pv001
  # labels: 可选标签，便于筛选和分组
  # labels:
  #   type: nfs
  #   env: test
spec:
  # ========== 容量配置 ==========
  capacity:
    # storage: PV 提供的存储大小，单位可以是 Mi、Gi、Ti
    # PVC 申请的 storage 必须 ≤ 此值才能绑定
    storage: 3Gi

  # ========== 卷模式 ==========
  # volumeMode: 卷的模式，可选值：
  #   - Filesystem: 文件系统模式（默认），Pod 挂载后是一个目录
  #   - Block: 块设备模式，Pod 挂载后是一个裸块设备（需 CSI 驱动支持）
  volumeMode: Filesystem

  # ========== 访问模式 ==========
  # accessModes: 定义 PV 的挂载方式，是一个列表
  # 可选值：
  #   - ReadWriteOnce (RWO): 单节点读写，只能被一个节点上的 Pod 挂载
  #   - ReadOnlyMany  (ROX): 多节点只读，可被多个节点同时挂载但只能读
  #   - ReadWriteMany  (RWX): 多节点读写，可被多个节点同时读写（NFS 支持）
  # 注意：列表项格式是 "- 值"，横线后面必须有空格！
  accessModes: 
    - ReadWriteOnce      # 这里修正了空格问题

  # ========== 回收策略 ==========
  # persistentVolumeReclaimPolicy: 当 PVC 被删除后，PV 如何处理
  # 可选值：
  #   - Retain: 保留，PV 变为 Released 状态，需管理员手动清理和复用
  #   - Delete: 删除，PV 和底层存储一起被删除（依赖后端存储支持）
  #   - Recycle: 已废弃！会执行 rm -rf / 后重新可用，不安全且不支持 NFS
  persistentVolumeReclaimPolicy: Retain   # 修正为 Retain

  # ========== 存储类 ==========
  # storageClassName: 关联的 StorageClass 名称
  # 作用：
  #   - PVC 只有指定相同的 storageClassName 才能绑定此 PV
  #   - 留空 "" 表示不使用任何 StorageClass（静态绑定）
  #   - 设为自定义值如 "manual"，可以区分不同的存储等级
  # 建议：测试环境直接留空 "" 或改为 "manual"
  storageClassName: manual    # 改为 manual，避免与不存在的 "slow" 混淆

  # ========== 挂载选项 ==========
  # mountOptions: 挂载时传递给 mount 命令的额外参数，仅对某些存储类型有效
  # NFS 常用选项：
  #   - hard: 客户端请求在超时后持续重试（推荐，避免数据丢失）
  #   - soft: 超时后返回错误（不推荐用于写入）
  #   - nfsvers=4.1: 指定 NFS 协议版本
  #   - rsize/wsize: 读写块大小，如 rsize=1048576,wsize=1048576
  #   - timeo=600: 超时时间（单位 0.1 秒）
  mountOptions:
    - hard
    - nfsvers=4.1
    # 也可以加上这些常用选项：
    # - rsize=1048576
    # - wsize=1048576
    # - timeo=600

  # ========== 后端存储定义（NFS） ==========
  # nfs: 指定 NFS 类型的后端存储
  nfs:
    # path: NFS 服务器上的共享目录路径（必须提前存在！）
    path: /data/nfs/rw/test-pv
    # server: NFS 服务器的 IP 地址或域名
    server: 192.168.6.100
    # readOnly: 可选，是否以只读方式挂载，默认 false
    # readOnly: false

