---
title: "Релиз Feather Wallet 2.0.0"
date: "2022-07-09"
categories:
  - "Новости"
tags:
  - "Feather Wallet"
thumbnail: ""  
lead: "Состоялось обновление минималистичного некастодиального кошелька Feather Wallet версии 2.0.0."
pager: true
toc: true
sidebar: "right"
widgets:
  - "recent"
  - "taglist"
---

_Эта версия уже готова к августовскому хардфорку._
_Ссылка для скачивания: https://featherwallet.org/download/_
_Сборки macOS и архив с исходным кодом уже на подходе. Эта версия будет доступна для автоматического обновления уже через несколько часов._

## _Особенности_

### _Новая схема мнемонической фразы Polyseed_

![01](/img/post/2022-07-09-feather-wallet-200-released/01.png)  
_Feather теперь генерирует мнемонические фразы, состоящие из 16, а не из 14 слов._

Библиотека monero-seed с доказательством концепции [мнемонической фразы](https://github.com/tevador/monero-seed) из 14 слов, которое Feather использовал до выхода версии 2.0.0, теперь устарела. Ей на смену приходит [Polyseed](https://github.com/tevador/polyseed). Мнемонические фразы из 14 слов более генерироваться не будут. Все новые кошельки будут использовать Polyseed. Обе схемы схожи с сточки зрения функционала. Подобно monero-seed, Polyseed имеет встроенную функцию даты создания кошелька. Удаление в данный момент не работает. Тем не менее поддержка восстановления мнемонических фраз, состоящих из 14 слов, будет осуществляться неограниченное количество времени. Нельзя преобразовать мнемоническую фразу из 14 слов в формат Polyseed. Чтобы перейти на Polyseed, необходимо создать новый кошелёк и перевести на него свои средства. Polyseed можно преобразовать в эквивалентную фразу из 25 слов + высоту восстановления в диалоговом окне Seed (Мнемоническая фраза), чтобы кошелёк можно было восстановить в других кошельках Monero.

### _Управление монетой: ручной выбор входов_

![02](/img/post/2022-07-09-feather-wallet-200-released/02.png)  
_Возможность ручного выбора входов, которые будут потрачены в транзакции. Эта функция является опциональной._

Ручной выбор входов гарантирует, что выходы будут потрачены в транзакции. Это удобно с точки зрения удаления «пыли» и других нежелательных выходов. Также это может быть полезно для тех пользователей, которые становятся жертвами нацеленных атак путём отслеживания с помощью «отравленных» выходов (и замораживания нежелательных). Также этим могут выгодно воспользоваться пользователи, объединяющиеся на продвинутом уровне или прибегающие к вспениванию.

### _Настройки: новая вкладка Privacy_

![03](/img/post/2022-07-09-feather-wallet-200-released/03.png)  
_Вкладка Privacy (Приватность) объединяет в себе все те настройки, которые могут повлиять на приватность пользователей._

Вкладка Privacy включает в себя самые разные новые функции:
- возможность отключения получения [данных третьих сторон](https://docs.featherwallet.org/guides/websocket);
- возможность отключения протоколирования данных на диск;
- возможность подключения офлайн-режима (все сетевые подключения отключаются);
- возможность установки параметра блокировки по бездействию.

### _Добавление вкладок на экран Home (Главный экран)_

![04](/img/post/2022-07-09-feather-wallet-200-released/04.png)  
_В этой версии экран Home (Главный экран) был дополнен следующим образом._

- добавлен тикер цены XMR/BTC;
- добавлена вкладка [bounties.monero.social](https://bounties.monero.social/);
- добавлена вкладка Revuo Monero.

В этой версии также была реализована поддержка Ledger Nano S+.

## _Улучшения_

- Возможность настройки офсета мнемонической фразы для фразы-пароля во время создания кошелька.
- Отправка: предупреждение о высокой комиссии.
- Возможность одновременного включения лишь одной копии Feather.
- Расчёты: предупреждения в случае получения неактуальных данных биржевого курса.
- Добавление WebSocket-узла с возможностью отката.
- libwallet: верификация ключей перед возвращением списка адресов.
- Предупреждение, если кошелёк не является детерминированным.
- Необходимость в указании пароля при использовании функции просмотра кэша кошелька (Wallet Cache Viewer).
- Tails: запись файлов в feather_data вместо .feather.
- Отправка: создание «разбитых» транзакций не допускается.
- Удалено диалоговое окно состояния соединения. Нажатие индикатора сети в строке состояния теперь открывает Settings -> Node (Настройки → Узел).
- Портативный режим: для включения портативного режима теперь принимается portable.txt (без точки в начале).
- Tails/Whonix: допускается соединение с не являющимися луковыми узлами в процессе синхронизации кошелька с целью её ускорения.

## _Исправления_

- Использован официальный исполняемый файл Tor Bundle для Windows (должен предотвратить отправку предупреждений Windows Defender).
  - Пользователям Windows более не придётся добавлять исключение для Tor в свой антивирус.
- macOS: исправлен набор разрешений для QR-сканера веб-камеры.
- Отправка: реализована возможность отправки всех средств в фиатном режиме.
- Исправлена ошибка, не позволявшая проводить транзакции в режиме тестовой/отладочной сети.
- Главная страница: переключение между вкладками теперь происходит без заметной задержки.
- Теперь все файлы, записанные Feather в портативном режиме, будут записываться в feather_data (включая ringdb).
- Исправлена ошибка, из-за которой настройки localMonero могли игнорироваться.
- Исправлено неверное сообщение об ошибке, появлявшееся во время подписания транзакции в офлайн-режиме.

## _Сопровождение_

- Сборки: прошедшие перекрёстную компиляцию и [воспроизводимые сборки macOS / сборки с возможностью начальной загрузки](https://github.com/feather-wallet/feather/pull/20) (WIP).
- Готовность Feather к переходу на Qt 6.
- Обновление Tor до версии 0.4.7.8.
- Обновление OpenSSL до версии 1.1.1p.
- Обновление Qt до версии v5.15.5.
- Удаление зависимости zeromq.
- macOS: возможность компиляции на машинах M1.

---

_Источник: [Feather Wallet 2.0.0 released](https://www.reddit.com/r/Monero/comments/vuinb0/feather_wallet_200_released/)_

_Перевод: [Mr. Pickles](https://t.me/v1docq47)_  
_Коррекция: [Kukima](https://t.me/Kukima)_