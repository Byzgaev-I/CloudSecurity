# Домашнее задание к занятию "Безопасность в облачных провайдерах"

## Задание 1. Yandex Cloud
1) С помощью ключа в KMS необходимо зашифровать содержимое бакета:  
 - создать ключ в KMS;    
 - с помощью ключа зашифровать содержимое бакета, созданного ранее.
2) (Выполняется не в Terraform)* Создать статический сайт в Object Storage c собственным публичным адресом и сделать доступным по HTTPS:    
 - создать сертификат;    
 - создать статическую страницу в Object Storage и применить сертификат HTTPS;    
 - в качестве результата предоставить скриншот на страницу с сертификатом в заголовке (замочек).    

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
```

#### 1.2. Шифрование содержимого бакета

Конфигурация в файле kms.tf:

```hcl
resource "yandex_kms_symmetric_key" "key-1" {
  name              = "storage-key"
  description       = "Key for storage encryption"
  default_algorithm = "AES_256"
  rotation_period   = "8760h"
}
```
### Настройка HTTPS для статического сайта

#### 2.1. Создание сертификата

Конфигурация в файле certificate.tf:
```hcl
resource "yandex_cm_certificate" "website_cert" {
  name    = "website-cert"
  domains = ["byzgaev-website-20250305.website.yandexcloud.net"]
  managed {
    challenge_type = "HTTP"
  }
}
```

#### 2.2. Настройка статического сайта

Конфигурация в файле storage.tf:
```hcl
resource "yandex_storage_bucket" "website" {
  bucket = "byzgaev-website-20250305"
  acl    = "public-read"

  website {
    index_document = "index.html"
    error_document = "error.html"
  }

  https {
    certificate_id = yandex_cm_certificate.website_cert.id
  }
}
```






































