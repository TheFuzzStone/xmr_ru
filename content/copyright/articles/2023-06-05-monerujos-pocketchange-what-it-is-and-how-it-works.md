---
title: "Monerujo PocketChange: что это, и как это работает?"
date: "2023-06-05"
categories:
  - "Статьи"
tags:
  - "Monerujo"
thumbnail: "img/copyright/articles/2023-06-05-monerujos-pocketchange-what-it-is-and-how-it-works/01.png"  
lead: "PocketChange - это самая последняя функция, добавленная в мобильный кошелёк Monerujo. Если в двух словах, когда эта функция включена, то когда вы тратите деньги, Monerujo создаёт дополнительные транзакции сдачи, и поэтому у вас всегда будут иметься разблокированные средства для последующей траты. Если это звучит несколько заумно и непонятно, не отчаивайтесь. Разберёмся во всём по порядку."
pager: true
toc: false
sidebar: "right"
---

## _Контекст_

Всякий раз, когда вы отправляете Monero (XMR) с одного адреса на другой, вы используете те монеты Monero, которые имеются в вашем кошельке. Тут всё понятно. Но эти монеты ([UTXO](https://en.wikipedia.org/wiki/Unspent_transaction_output)) отличаются от своих физических аналогов забавным образом: у них может быть любой номинал. Например, вы можете иметь монету XMR номиналом 3,9857648773, тогда как в реальном мире вы всегда будете ограничены фиксированной суммой, скажем, 50 центами.

Поэтому, когда вы хотите отправить Monero, ваш кошелёк проверяет имеющиеся у вас монеты и выбирает, какую из них потратить. Но в 99,99% случаев сумма, которую вы хотите отправить, не совпадает с суммой любой из тех монет, которые у вас имеются. Поэтому ваш кошелёк разделяет эту монету и совершает две транзакции: одна с суммой, которую вы хотите отправить, а другая со сдачей (той частью монеты, которую вы не хотите отправлять), которая вернётся обратно к вам. Это так же, как если бы вы заплатили за кофе стоимостью 3 евро купюрой в 5 евро и получили бы в качестве сдачи две монеты по 1 евро.

![02](/img/copyright/articles/2023-06-05-monerujos-pocketchange-what-it-is-and-how-it-works/02.png)
_Ваш кошелёк постоянно «рвёт купюры» на части, но вы этого просто не замечаете._

## _Проблема_

У Monero весьма особенный... назовём это словом «дизайн». Вы заметили, что когда вы получаете Monero, монеты некоторое время остаются заблокированными? Вы видите, что монеты пришли, но они не добавляются к балансу вашего кошелька. Так происходит потому, что в Monero действует правило, согласно которому никто не может отправлять полученные Monero снова, пока не будет вычислено 10 новых блоков. Поскольку сеть Monero настроена так, что новые блоки появляются ней в среднем каждые 2 минуты, на вычисление этих 10 блоков обычно уходит 20 минут.

Возникает вопрос: почему, во имя всех криптовалютных богов, в Monero есть такое раздражающее и громоздкое ограничение? Ответ заключается в [безопасности](https://github.com/monero-project/research-lab/issues/95). Предполагается, что реорганизация блокчейна глубиной в 10 блоков слишком затратна, чтобы осуществлять её. Поэтому если по истечении 10 блоков у вас всё ещё есть ваши монеты, вы (и все остальные) можете быть уверены, что эти Monero точно ваши, и вы можете использовать их по своему усмотрению. Эти 10 блоков являются своего рода чистилищем для монет Monero — они уже ваши, но вы пока не можете их использовать.

![03](/img/copyright/articles/2023-06-05-monerujos-pocketchange-what-it-is-and-how-it-works/03.png)
_Monero защищает от необдуманных трат. Спасибо, Monero!_

Если мы используем Monero в качестве валюты в реальной жизни (а мы должны это делать), то это действительно проблема. Представьте, что вы находитесь на конференции Monero и платите за пиво в баре. Вам придётся ждать 20 минут, прежде чем вы сможете купить к нему арахис. К тому времени, когда ваша сдача будет разблокирована, вы уже либо выпьете пиво, либо оно станет тёплым. Или же, допустим, к вам подойдёт привлекательная девушка, но вы не сможете купить ей напиток, пока не пройдёт 20 минут. К этому моменту вы либо уже будете целоваться, либо дама поймёт, сколько у вас неразрешённых детских комплексов.

## _Решение_

Безусловно, есть способ избежать всего этого: носить с собой несколько кошельков. Но в Монероленде это довольно обременительно, поскольку, чтобы воспользоваться ими, вам придётся синхронизировать их все по отдельности. А это тот ещё геморрой.

![04](/img/copyright/articles/2023-06-05-monerujos-pocketchange-what-it-is-and-how-it-works/04.png)
_Буквально._

Более умный (ну или занудный) способ решения этой проблемы — отправлять Monero самому себе, чтобы разбить монеты. Но если вы не разобьёте эти монеты вручную на разные транзакции, которые пойдут на разные адреса, ваш кошелёк попытается снова объединить их вместе. Именно так вам и придётся действовать, если ваш кошелёк поддерживает отправку на несколько адресов в одной транзакции. Это работает, но вы должны всё тщательно продумать, рассчитать суммы, скопировать и вставить разные адреса, и сделать всё это заранее, иначе не будет никакой разницы. Короче говоря, это возможно, но это «хакерство четвёртого уровня».

## _Встречайте PocketChange!_

Если вы включите PocketChange то Monerujo будет делать... именно это! Кошелёк всё сделает за вас, и вам не придётся ничего вычислять, а также вы получите все преимущества без каких-либо усилий (и сертификата «хакера 4-го уровня»).

Пока эта функция будет включена, всякий раз, когда вы будете использовать Monerujo для отправки Monero, кошелёк будет брать большую монету, делить её на части и распределять получившиеся маленькие монеты по 10 разным карманам. Таким образом, монеты не будут сливаться обратно, и вы сможете мгновенно потратить деньги из любого из этих карманов, не дожидаясь, пока истекут утомительные 20 минут.

![05](/img/copyright/articles/2023-06-05-monerujos-pocketchange-what-it-is-and-how-it-works/05.png)
_Поясные сумки снова в моде. Кто бы мог подумать?_

В чем же минус, спросите вы? Ну, поскольку в Monero вы платите майнерам за транзакции, которые они записывают в блокчейн, если им приходится записать больше, потому что вы отправляете несколько монет в несколько мест, они будут брать больше. Но поскольку Monero решила использовать динамический размер блоков и, следовательно, низкие комиссии, даже если вы платите в 4 раза больше обычной комиссии, это всё равно составит доли цента. Скажем так, вас это не сильно будет волновать, если вы хотите тратить Monero как никогда раньше.

Как это сделать? Просто откройте кошелёк Monerujo и зайдите в меню. Нажмите PocketChange. Вы увидите флажок включения и выключения, а также ползунок для выбора размера монет, которые будут генерироваться для каждого кармана. Если сомневаетесь, оставьте значение 0,1 XMR. Это означает, что вы сможете купить что-то стоимостью менее 14 евро 10 раз (или даже больше) менее чем за 20 минут. Если вы решите, что вам это не нужно, и захотите максимально снизить размер выплачиваемых комиссий, просто отключите функцию. Важно понимать, что вы сможете воспользоваться этим преимуществом уже после первой отправки с включенной функцией.

## _Как это работает_

Если вы дочитали до этого момента, значит, вы из тех людей, которые предпочитают разбираться во всём. Искренне рад за вас! Когда функция PocketChange включена, Monerujo берёт первые 10 подадресов счёта этого кошелька и использует их в качестве карманов. Поскольку баланс каждого счёта индивидуален, то и эта настройка тоже (вы можете иметь только один выделенный счёт PocketChange, не связанный с остальными).

В следующий раз, когда вы будете тратить средства, кошелёк проверит ваши 10 подадресов на наличие 0,1 XMR (или любой другой заданной вами суммы). Если нужная сумма будет найдена, то будет проведена обычная транзакция. Если нет, кошелёк произведёт столько монет сдачи, сколько нужно, чтобы заполнить их.

![06](/img/copyright/articles/2023-06-05-monerujos-pocketchange-what-it-is-and-how-it-works/06.png)
_Да прольётся денежный дождь!_

Вы можете узнать, будет ли транзакция, которую вы собираетесь совершить, транзакцией PocketChange. Это будет указано на последнем экране при отправке. Конкретней, там будет указано, какую сумму вы отправляете на указанный вами адрес, сколько составляет размер комиссии и сколько составляет сумма UTXO, которую вы отправляете обратно себе в качестве сдачи.

Если вы захотите проверить это, всё можно будет увидеть на экране Details (подробности транзакции). Вы сможете увидеть каждую отдельно взятую транзакцию и её идентификатор TX ID. Заморачивайтесь, если вам угодно.

## _Подведение итогов_

Печально известный 20-минутный рефрактерный период ― это то, с чем мы, любители Monero, привыкли мириться. Это неудивительно, и иногда нам хватает предусмотрительности подготовиться к этому заранее. Но Monerujo задуман как кошелёк, которым вы сможете воспользоваться буквально на улице. Мы мечтаем, чтобы Monero ежедневно использовалась в качестве наличных, а Monerujo ― в качестве повседневного кошелька. Мы не должны ждать так долго, чтобы заплатить за мясо после покупки овощей или чего-то другого, что вам больше всего по вкусу.

Сегодня это ограничение ощущается в основном в редких (но прекрасных) случаях, таких как [Конференция Monero](https://monerokon.com/), где мы можем [день или два поиграть](https://monerotopia.com/) в страну, где можно платить за любые вещи нашей любимой монетой. Но инструменты должны быть готовы до того, как они понадобятся, иначе мы никогда к этому не придем.

Разработка этой функции финансировалась нашими любимыми пользователями. Вы можете увидеть её вместе с другими идеями, некоторые из которых уже реализованы, а некоторые ещё в процессе реализации, на нашей странице [funding.monerujo.app](https://funding.monerujo.app). Спасибо всем тем, кто поддерживает нас!

![07](/img/copyright/articles/2023-06-05-monerujos-pocketchange-what-it-is-and-how-it-works/07.png)

---

_Источник: [Monerujo’s PocketChange: what it is and how it works](https://anhdres.medium.com/monerujos-pocketchange-what-it-is-and-how-it-works-8e1ea1f7489e)_

_Перевод: [Mr. Pickles](https://t.me/v1docq47)_  
_Коррекция: [Kukima](https://t.me/Kukima)_