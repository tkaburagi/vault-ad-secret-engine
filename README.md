## setup

```
docker run \
  --name vault-openldap \
  --env LDAP_ORGANISATION="learn" \
  --env LDAP_DOMAIN="learn.example" \
  --env LDAP_ADMIN_PASSWORD="2LearnVault" \
  -p 389:389 \
  -p 636:636 \
  --detach \
  --rm \
  osixia/openldap:latest
```

```
ldapadd -cxWD "cn=admin,dc=learn,dc=example" -f learn-vault-example.ldif
```

```
vault write openldap/config \
    binddn=cn=admin,dc=learn,dc=example \
    bindpass=2LearnVault \
    url=ldap://127.0.0.1
```

```
vault write openldap/static-role/learn \
    dn='cn=alice,ou=users,dc=learn,dc=example' \
    username='alice' \
    rotation_period="1m"
```

## demo

```
vault read openldap/config
```

```
vault read openldap/static-role/learn
```
```
vault read openldap/static-cred/learn
```

```
ldapsearch -b "cn=alice,ou=users,dc=learn,dc=example" \
    -D 'cn=alice,ou=users,dc=learn,dc=example' \
    -w (PASSWORD)
```