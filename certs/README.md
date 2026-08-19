# certs/

存放原厂 GKI 内核模块签名公钥。构建时 `build.yml` 会自动检测本目录下的
`stock_gki_key.pem`：存在则把公钥编入内核信任链（`CONFIG_SYSTEM_TRUSTED_KEYS`），
使内核可以加载原厂/厂商签名的内核模块；不存在则自动跳过，不影响构建。

## 使用方法

1. 获取原厂模块公钥（X.509 PEM 证书，内容形如 `-----BEGIN CERTIFICATE----- ... -----END CERTIFICATE-----`）。
2. 将文件命名为 `stock_gki_key.pem` 放到本目录。
3. 提交推送后构建即可，无需其他配置。

## 如何获取公钥

- 从原厂 boot 镜像的内核 Image 中提取证书：

  ```bash
  strings Image | grep -A 30 'BEGIN CERTIFICATE'
  ```

- 或从设备 vendor 分区内的原厂 .ko 模块签名中提取证书。

> 保存为 PEM 时务必包含完整的 `BEGIN CERTIFICATE` / `END CERTIFICATE` 块。