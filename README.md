# Домашнее задание к занятию "Безопасность в облачных провайдерах"

## Задание 1. Yandex Cloud

### 1. Шифрование бакета с помощью KMS

#### 1.1. Создание ключа в KMS

Файл kms.tf:
```hcl
resource "yandex_kms_symmetric_key" "key-1" {
  name              = "storage-key"
  description       = "Key for storage encryption"
  default_algorithm = "AES_256"
  rotation_period   = "8760h"
}
