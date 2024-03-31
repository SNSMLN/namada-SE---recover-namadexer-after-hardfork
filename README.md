# namada-SE---recover-namadexer-after-hardfork

Во время хардфорка на высоте 237907, при обновлении базы namada , по непонятной причине, из базы были удалены индексы. Соотвественно синхронизировать namadexer с 0 не получится. Есть два пути восстановления. 

During the hard fork at height 237907, when updating the namada database, for some unknown reason, indexes were removed from the database. Accordingly, it will not be possible to synchronize namadexer with 0. There are two ways to recover.

**1й - долгий**. Порядка 2х дней. Нужно запустить временную ноду namada с состоянием до хардфорка, и синхронизировать ее с 0 до 237907. Синхронизировать namadexer до высоты 237907. Переключиться в конфиге namadexer на актуальную сеть namada , после хардфорка. И завершить синхронизацию базы. 

**1st - long**. About 2 days. You need to launch a temporary namada node with the state before the hard fork, and synchronize it from 0 to 237907. Synchronize namadexer to a height of 237907. Switch in the namadexer config to the current namada network, after the hard fork. And complete database synchronization.

**2й -быстрый**. Примерно 10 минут. Просто восстановить базу данных из дампа.

**2nd - fast**. About 10 minutes. Simply restore the database from a dump.

Оба варианта для версии namadexer 1.0.1

Both options for namadexer version 1.0.1



**1 вариант**

**1 option**

останавливаем индексер

stop the indexer

```sudo systemctl stop namadexer-indexerd```

удаляем существующую базу postgresql

delete the existing postgresql database

```sudo -u postgres psql -U postgres -c "drop database blockchain"```

создаем новую базу postgresql

create a new postgresql database

```sudo -u postgres createdb blockchain```

В конфигурации namadexer изменяем адрес ноды namada на временно запущенную ноду намады, с состоянием до хардфорка. Можно запустить свою. Можно использовать чужую ноду , спасибо @BlackOreo.

In the namadexer configuration, we change the address of the namada node to a temporarily running namada node, with the state before the hard fork. You can launch your own. You can use someone else's node, thanks @BlackOreo.

```--indexer-tendermint-addr http://51.159.167.32:26657 ```

запускаем индексер

start the indexer

```sudo systemctl restart namadexer-indexerd```

Ждем синхронизацию до 237907 блока. Останавливаем индексер, В конфигурации namadexer возвращаем обратно свою локальную ноду namada

Waiting for synchronization to block 237907. stop the indexer, in the namadexer configuration we return back our local node namada

```--indexer-tendermint-addr http://127.0.0.1:26657```

запускаем индексер, ждем синхронизацию до актуального блока.

start indexer, wait for synchronization to the current block.



**2 вариант** 

**Option 2**

скачиваем дамп базы postgresql

download postgresql database dump

```wget https://namada.snsmln.xyz/blockchain.dump.tar```

останавливаем индексер

stop the indexer

```sudo systemctl stop namadexer-indexerd```

удаляем существующую базу postgresql

delete the existing postgresql database

```sudo -u postgres psql -U postgres -c "drop database blockchain"```

создаем новую базу postgresql

create a new postgresql database

```sudo -u postgres createdb blockchain```

Восстанавливаем базу из дампа

Restoring the database from a dump

```sudo -u postgres pg_restore --dbname blockchain --format=tar blockchain.dump.tar```


запускаем индексер, ждем синхронизацию до актуального блока.

start indexer, wait for synchronization to the current block.
