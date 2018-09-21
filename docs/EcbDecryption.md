**ВАЖНО!** Перед данной работой рекомендуется выполнить 
[лабораторную на распознание режимов шфирования.](https://github.com/CryptoCourse/CryptoLabs/blob/master/docs/labEncryptionModeDist.md)

Дана REST служба с API указанным ниже.

Служба шифует указанный пользователем открытый текст, следующим образом:

`E(k, random_padding || user_data || target_data||)`

`random_padding` - случайное дополнение, фиксированное для пары `<userId>`, `<challengeId>`

`user_data` - данные пользователя (из post запроса)

`target_data` - целевые данные, фиксированные для пары `<userId>`, `<challengeId>`.

Задача - расшифровать target_data.

**ВАЖНО!** Все передаваемые сообщения должны быть предварительно закодированы с помощью BASE64. 

Полученные сообщения так же должны быть декодированы из BASE64 в указанный тип.

**ВАЖНО!** Все строки кодируется с использование кодировки ASCII.

т.е. передача строки для лабы выглядит следующим образом:
строка -> (asсii) -> массив байт -> (base64) -> строка -> json 

## Ход работы.

### Тестирование 

`<userId>` = имя аккаунта на GitHub  (или фамилия студента)

`<challengeId>` = 1, 2, 3, 4, 5

1. Проверить работоспособность контроллера с помощью метода `GET <host>/api/EcbDecryption`
2. Получить зашифрованное сообщение с помощью метода `POST <host>/api/EcbDecryption/<userId>/<challengeId>/noentropy`
3. Определить режим шифрования
3.1 Если режим шифрования CBC, перейти к следующему `<challengeId>`.
4. Используя метод `POST <host>/api/EcbDecryption/<userId>/<challengeId>/noentropy` получить необходимые шифртексты.
5. Расшифровать target_data
6. Проверить верность ответа использовав метод `GET <host>/api/EcbDecryption/<userId>/<challengeId>/verify`
7. Проверить корреиность программы для `<challengeId>` = 1..10

### Сдача лабы
шаги 1 - 7 этапа тестирования аналогично, но для 15 различных `<challengeId>`.

## Описание API

Rest запросы, в заголовке выстален Content-Type: application/json; charset=utf-8.

### Описание методов

## `GET <host>/api/EcbDecryption`

Проверка работоспособности контроллера. Возращает `operating`. Ответ не кодируется в BASE64.

| Парметр| Описание| 
| --- | --- 
| `<host>` | имя хоста веб службы


## `POST <host>/api/EcbDecryption/<userId>/<challengeId>/noentropy`

Дополняет полученные данные случайными байтами в начале и в конце. Способ дополнения описан в начале лабораторной.

После чего зашифровывает данные на фиксированном для задания ключе, либо в режиме CBC, либо в ECB.

| Парметр| Описание| 
| --- | --- 
| `<host>` | имя хоста веб службы
| `<userId>` | идентификатор студента
| `<challengeId>` | идентификатор задания

## `GET <host>/api/EncryptionModeOracle/<userId>/<challengeId>/verify`

Возвращает target_data для указанного задания

| Парметр| Описание| 
| --- | --- 
| `<host>` | имя хоста веб службы
| `<userId>` | идентификатор студента
| `<challengeId>` | идентификатор задания