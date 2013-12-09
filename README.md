## Openshift Quickstart - Magento + Nginx + Redis

Crie sua aplicação.

```
$ rhc app create -s magento mysql-5 https://reflector-getupcloud.getup.io/reflect?github=caruccio/openshift-nginx-php-fpm https://reflector-getupcloud.getup.io/reflect?github=smarterclayton/openshift-redis-cart
$ cd magento
```

Adicione esse quickstart ao seur repositório.

```
$ git remote add upstream https://github.com/caruccio/openshift-magento.git
$ git pull -s recursive -X theirs upstream master
```

Edite `php/app/etc/local.xml.erb` e troque a chave `config/global/crypt/key` para um novo valor.
Voce pode usar o comando `openssl` para gerar uma nova chave. Copie e cole o resultado do comando abaixo
no seu arquivo `local.xml.erb` (obs: substitua apenas o conteudo de `CDATA[...]`).

```
$ openssl rand -base64 33
cy2Ew2HLmmOWRF1YlvabqD40sA7UuCa929p/rMRjwRrr
```

Por exemplo, meu arquivo ficou assim:

```
...
        <crypt>
            <key><![CDATA[cy2Ew2HLmmOWRF1YlvabqD40sA7UuCa929p/rMRjwRrr]]></key>
        </crypt>
...
```

Agora faça o deploy. (Obs: este passo pode demorar)

```
$ git commit -a -m 'Update secret key'
$ git push
```

Acesse https://magento-$namespace.getup.io/admin/ para configurar o Magento.

```
Username: admin
Password: OpenShiftAdmin123
```

## Configurações recomendadas

`System` -> `Configuration` -> `Menu Advanced` -> `System`: troque o campo
`Storage Configuration for Media` para `Database`, clique em `Synchronize` e depois `Save Config`.

`System` -> `Cache Management`: clique em `Select All`, selecione `Actions` -> `Enable` e clique em `Submit`.
