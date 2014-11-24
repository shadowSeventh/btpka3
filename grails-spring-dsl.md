# 使用factory方法创建bean

```groovy
import org.apache.curator.framework.CuratorFrameworkFactory
import org.apache.curator.retry.ExponentialBackoffRetry

beans = {
    def cfg = application.config
    // ZooKeeper相关配置
    zkRetry(ExponentialBackoffRetry, 1000, 3)
    zkClient(CuratorFrameworkFactory){bean ->
        bean.factoryMethod = "newClient"
        bean.constructorArgs = [cfg.lizi.zk.connStr, ref('zkRetry')]
        bean.initMethod = 'start'
        bean.destroyMethod = 'close'
    }
}
```