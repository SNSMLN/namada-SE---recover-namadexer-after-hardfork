# namada-SE---recover-namadexer-after-hardfork

Во время хардфорка на высоте 237907, при обновлении базы namada , по непонятной причине, из базы были удалены индексы. Соотвественно синхронизировать namadexer с 0 не получится. Есть два пути восстановления. 

**1й - долгий**. Но в unix style. Порядка 2х дней. Нужно запустить временную ноду namada с состоянием до хардфорка, и синхронизировать ее с 0 до 237907. Синхронизировать namadexer до высоты 237907. Переключиться в конфиге namadaxer на актуальную сеть namada , после хардфорка. И завершить синхронизацию базы. 

**2й -быстрый**. Примерно 10 минут. Просто восстановить базу данных из дампа.

Оба варианта для версии namadexer 1.0.1


**1 вариант** 

останавливаем индексер
```sudo systemctl stop namadaxer-indexerd```

останавливаем postgresql
```sudo systemctl restart postgresql```

удаляем существующую базу postgresql
```sudo -u postgres psql -U postgres -c "drop database blockchain"```

создаем новую базу postgresql
```sudo -u postgres createdb blockchain```

В конфигурации namadexer изменяем адрес ноды namada на временно запущенную ноду намады, с состоянием до хардфорка. Можно запустить свою. Можно использовать мою.
```--indexer-tendermint-addr https://namada-rpc-237907.snsmln.xyz:443 ```

запускаем индексер
```sudo systemctl restart namadaxer-indexerd```

Ждем синхронизацию до 237907 блока

останавливаем индексер, В конфигурации namadexer возвращаем обратно свою локальную ноду namada
```--indexer-tendermint-addr https://65.108.204.205:16657```

запускаем индексер, ждем синхронизацию до актуального блока.


**2 вариант** 


скачиваем дамп базы postgresql
```wget https://namada.snsmln.xyz/blockchain.dump.tar```

останавливаем индексер
```sudo systemctl stop namadaxer-indexerd```

удаляем существующую базу postgresql
```sudo -u postgres psql -U postgres -c "drop database blockchain"```

создаем новую базу postgresql
```sudo -u postgres createdb blockchain```

Восстанавливаем базу из дампа
```sudo -u postgres pg_restore --dbname blockchain --format=tar blockchain.dump.tar```

