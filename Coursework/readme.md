## **Процесс установки и настройки ufw**
![Screenshot](1.jpg)
## **Процесс установки и выпуска сертификата с помощью hashicorp vault**
### **Запускаем сервер hashicorp vault, назначаем переменные**
```
export VAULT_TOKEN=s.ACEImTJkFrwVjY8mPmfbHupE
export VAULT_ADDR=http://127.0.0.1:8200
vault server -dev -dev-root-token-id $VAULT_TOKEN
```
### **Генерируем корневой (название CA_localhost) сертификат**
```
curl --header "X-Vault-Token: $VAULT_TOKEN" \
   --request POST \
   --data '{"type":"pki"}' \
   $VAULT_ADDR/v1/sys/mounts/pki

curl --header "X-Vault-Token: $VAULT_TOKEN" \
   --request POST \
   --data '{"max_lease_ttl":"87600h"}' \
   $VAULT_ADDR/v1/sys/mounts/pki/tune

tee payload.json <<EOF
{
  "common_name": "CA_localhost",
  "ttl": "87600h"
}
EOF

curl --header "X-Vault-Token: $VAULT_TOKEN" \
   --request POST \
   --data @payload.json \
   $VAULT_ADDR/v1/pki/root/generate/internal \
   | jq -r ".data.certificate" > CA_cert_new.crt

tee payload-url.json <<EOF
{
  "issuing_certificates": "$VAULT_ADDR/v1/pki/ca",
  "crl_distribution_points": "$VAULT_ADDR/v1/pki/crl"
}
EOF

curl --header "X-Vault-Token: $VAULT_TOKEN" \
   --request POST \
   --data @payload-url.json \
   $VAULT_ADDR/v1/pki/config/urls
```  
### **Генерируем промежуточный сертификат (название localhost SPB for Netology)**
``` 
curl --header "X-Vault-Token: $VAULT_TOKEN" \
   --request POST \
   --data '{"type":"pki"}' \
   $VAULT_ADDR/v1/sys/mounts/pki_int

curl --header "X-Vault-Token: $VAULT_TOKEN" \
   --request POST \
   --data '{"max_lease_ttl":"43800h"}' \
   $VAULT_ADDR/v1/sys/mounts/pki_int/tune

tee payload-int.json <<EOF
{
  "common_name": "localhost SPB for Netology"
}
EOF

curl --header "X-Vault-Token: $VAULT_TOKEN" \
   --request POST \
   --data @payload-int.json \
   $VAULT_ADDR/v1/pki_int/intermediate/generate/internal | jq -r  '.data.csr' > pki_int.csr

cat pki_int.csr | jq --raw-input --slurp --compact-output >test.csr
pki_full=$(<test.csr)

tee payload-int-cert.json <<EOF
{
  "csr": $pki_full,
  "format": "pem_bundle",
  "ttl": "43800h"
}
EOF

curl --header "X-Vault-Token: $VAULT_TOKEN" \
   --request POST \
   --data @payload-int-cert.json \
   $VAULT_ADDR/v1/pki/root/sign-intermediate | jq -r  '.data.certificate' > cert.crt

cat cert.crt | jq --raw-input --slurp --compact-output >test.crt
value_certificate=$(<test.crt)

tee payload-signed.json <<EOF
{
  "certificate": $value_certificate
}
EOF

curl --header "X-Vault-Token: $VAULT_TOKEN" \
     --request POST \
     --data @payload-signed.json \
     $VAULT_ADDR/v1/pki_int/intermediate/set-signed
``` 
### **Создаём роль для настройки параметров и контроля сертификатов**
``` 
tee payload-role.json <<EOF
{
  "allowed_domains": "localhost",
  "allow_subdomains": true,
  "max_ttl": "720h"
}
EOF

curl --header "X-Vault-Token: $VAULT_TOKEN" \
    --request POST \
    --data @payload-role.json \
    $VAULT_ADDR/v1/pki_int/roles/example-dot-com
```
### **Запрашиваем сертификат и преобразуем полученные данные в промежуточный сертификат, полученный по запросу сертификат и приватный ключ**
``` 
curl --header "X-Vault-Token: $VAULT_TOKEN" \
    --request POST \
    --data '{"common_name": "localhost", "ttl": "720h"}' \
    $VAULT_ADDR/v1/pki_int/issue/example-dot-com | jq > test.localhost.crt 

cat test.localhost.crt | jq -r .data.certificate > localhost.crt
cat test.localhost.crt | jq -r .data.issuing_ca >> localhost.crt
cat test.localhost.crt | jq -r .data.private_key > localhost.key
```
## **Процесс установки и настройки сервера nginx**

## **Страница сервера nginx в браузере хоста не содержит предупреждений**

## **Скрипт генерации нового сертификата**

## **Crontab работает**
