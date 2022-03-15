---
title: "Охота на логические ошибки в Monero"
date: "2021-03-24"
categories:
  - "Статьи"
tags:
  - ""
lead: "В течение последних десяти лет нам посчастливилось принять участие в разработке программного обеспечения для нескольких открытых проектов в криптовалютном пространстве. Но совсем недавно мы осознали, что весь полученный нами опыт также мог бы пригодиться в области анализа безопасности."
pager: true
toc: true
sidebar: "right"
---

## _Вступление_

Новые способы применения технологий в криптовалютном пространстве появляются с невероятной скоростью, но такая скорость наблюдается только на поверхности. Более глубокие, основополагающие, ключевые концепции блокчейн-технологии развиваются в куда более умеренном темпе.

Многие криптовалюты используют одну и ту же кодовую базу и зачастую попросту являются клонами Bitcoin, Ethereum, Monero и так далее. Опыт, получаемый в ходе работы над криптовалютным проектом, легко можно применить к другому проекту, поскольку они либо будут иметь много общего, либо (в некоторых случаях) будут использовать одинаковый код.

Нами было принято решение потратить несколько месяцев на изучение кодовых баз, поиск ошибок и выявление возможного влияния таких ошибок на безопасность в условиях реального мира. И это путешествие оказалось весьма захватывающим, поскольку нам удалось найти ошибки во всех затронутых нами проектах. К счастью, далеко не все эти ошибки представляли собой риск с точки зрения безопасности.

Это история ошибки, которая с высокой степенью вероятности могла быть использована в дурных целях, но оказалась пустышкой. Нам кажется, что предлагаемый нами материал стоит прочтения. Его легко понять, а методология, описанная ниже, достаточно универсальная для того, чтобы воспользоваться ею при проведении собственных исследований.

## _Методология_

Мы решили охватить максимально возможное количество проектов за период времени, который был выделен для этого исследования. То есть на изучение каждого проекта у нас было всего по несколько дней. Кодовая база Monero слишком велика для того, чтобы мы могли проанализировать её в пределах установленных нами временных рамок, поэтому мы выбрали вариант, представлявшийся нам наиболее интересным. Мы остановились на классах блокчейна и их функциях, поскольку именно они потенциально допускают возможность двойной траты и могут стать собственным «принтером денег» (money printer).

Прежде всего было необходимо всерьёз разобраться с самой сущностью кодовой базы. На удивление база имела гораздо больше схожего с кодовой базой Bitcoin, чем ожидалось. Был скомпилирован и проверен список интересных классов и их функций, затем был проведён высокоуровневый анализ того, что должен выполнять код и какие допуски в этом случае были предусмотрены, и наконец мы провели мозговой штурм с целью выявления возможных векторов атаки.

`Blockchain`: отвечает за добавление блока на верхнем уровне.
- `Blockchain::handle_block_to_main_chain(...)`: можно ли обмануть алгоритм доказательства работы (PoW)?
- `Blockchain::check_tx_inputs(...)`: можно ли избежать проверки входов какой-либо транзакции?
- `Blockchain::pop_blocks(...)`: можно ли сделать так, чтобы отбрасывались «законные» блоки?

`BlockchainDB`: отвечает за логику общей базы данных:
- `BlockchainDB::add_block(...)`: можно ли добавить неправильные блоки, позволяющие осуществлять двойную трату?
- `BlockchainDB::pop_block(...)`: можно ли отбросить блоки, содержащие собственные транзакции?
- `BlockchainDB::add_transaction(...)`: можно ли вызвать неправильное поведение при добавлении транзакции?
- `BlockchainDB::remove_transaction(...)`: можно ли случайно переместить транзакцию?

`BlockchainLMDB`: отвечает за логику определённой базы данных LMBD:
- `BlockchainLMDB::add_spent_key(...)`: можно ли сделать так, чтобы ключ траты не сохранялся в базе данных?
- `BlockchainLMDB::remove_spent_key(...)`: можно ли каким-либо образом удалить свой ключ траты после того, как он был потрачен?

Итак, причина, по которой для анализа был выбран именно этот код, состоит в том, что опыт предыдущей работы свидетельствует о прохождении сложного криптографического кода очень строгого аудита, но сам по себе код, предназначенный для обработки транзакций, такому аудиту не подвергается. Тем не менее такой код очень важен и может содержать ошибки, которые будут иметь те же последствия, что и взлом криптографической защиты.

## _Первые тревожные признаки_

```
bool Blockchain::handle_block_to_main_chain(...)
{
    ...
    // FIXME: the storage should not be responsible for validation.
    //        If it does any, it is merely a sanity check.
    //        Validation is the purview of the Blockchain class
    //        - TW
    //
    // ND: this is not needed, db->add_block() checks for duplicate k_images
    // and fails accordingly.
    // if (!check_for_double_spend(tx, keys))
    // {
    //     LOG_PRINT_L0("Double spend detected in transaction (id: " << tx_id);
    //     bvc.m_verifivation_failed = true;
    //     break;
    // }
    ...
}
```

Как правило, код, позволяющий осуществить двойную трату, можно обнаружить путём простого поиска соответствующих слов в кодовой базе. Это вывело нас на этот милый комментарий, согласно которому код, не допускающий возможность двойной траты, был отключён в целях оптимизации, так как проводилась проверка чего-то другого.

Поиск в кодовой базе причины, по которой двойная трата невозможна, был недолгим, но мы всё же сэкономим ваше время:

```
BlockchainDB::add_block(...)
  -> BlockchainDB::add_transaction(...)
    -> BlockchainLMDB::add_spent_key(...)
```

В конечном счёте база данных `LMDB` и соответствующая функция `BlockchainLMDB::add_spent_key(...)` сгенерируют исключительную ситуацию. При попытке добавления ключа траты в базу данных база данных проверяет, не существовал ли этот ключ раньше, и генерирует исключительную ситуацию.

Но дальнейший анализ показал, что функция `BlockchainDB::add_transaction(...)` проявляет некоторые признаки иного поведения, о чём будет сказано в следующем разделе.

Стоит отметить, что в случае с Monero, в отличие от других публичных блокчейнов (Bitcoin, Ethereum и т. д.), только отправителю на самом деле известно, был потрачен вход или нет. Механизм защиты от двойной траты предполагает, что каждый потраченный вход получает уникальный ключ, обычно называемый образом ключа. Только владелец входа может подписать транзакции со своим входом, генерируя при этом соответствующий образ ключа. Такие ключи сохраняются навсегда и служат защитой от двойной траты.

Если входящая транзакция содержит вход, который уже был записан в базе данных, транзакция отклоняется как попытка двойной траты.

## _Неправильные вложенные циклы for-loop_

Код, предлагаемый вашему вниманию ниже, является примером того, что мы ищем при проведении аудитов. Это цикл for-loop, который представляет собой небольшой кусок кода, но, если что-то идёт не так, как ожидается, за этим циклом следует другой for-loop, аннулирующий всё выполненное.

```
void BlockchainDB::add_transaction(...)
{
  ...
  for (const txin_v& tx_input : tx.vin)
  {
    if (tx_input.type() == typeid(txin_to_key))
    {
      add_spent_key(boost::get<txin_to_key>(tx_input).k_image);
    }
    else if (tx_input.type() == typeid(txin_gen))
    {
      /* nothing to do here */
      miner_tx = true;
    }
    else
    {
      LOG_PRINT_L1("Unsupported input type, removing key images and aborting transaction addition");
      for (const txin_v& tx_input : tx.vin)
      {
        if (tx_input.type() == typeid(txin_to_key))
        {
          remove_spent_key(boost::get<txin_to_key>(tx_input).k_image);
        }
      }
      return;
    }
  }
  ...
```

В некоторых случаях это довольно чёткий шаблон, но в конкретно в данном примере это равносильно бомбе замедленного действия.

Следует отметить, что код повторяет цикл по входам и запросам `add_spent_key` до тех пор, пока он не сможет распознать тип входа, после чего он удалит все образы ключей всех входов, даже тех входов, которые могли быть и не добавлены в базу данных в первую очередь.

Это и является ответом на вопрос, можно ли в процессе обработки транзакции B удалить ключ траты, добавленный в предыдущей транзакции A? Если кратко, то нет, но давайте выясним, почему это взаимосвязано!

## _В попытке изготовить эксплойт_

Работа нашего ошибочного кода во многом зависит о того, какой тип входа обрабатывается, и, если мы получаем транзакцию, содержащую тип входа, который код не может распознать, только в этом случае будут удалены все образы ключей траты в транзакции, в результате чего можно будет успешно провести атаку путём двойной траты.

Итак, какие же типы входов возможны? Проводим быстрый поиск в кодовой базе (CTRL+F нам в помощь):
- `txin_to_key`
- `txin_gen`
- `txin_to_script`
- `txin_to_scripthash`

Ух ты! Ещё два типа входов: `txin_to_script` и `txin_to_scripthash`. Сперва это выглядело многообещающе, и, поверьте, мы очень обрадовались, увидев это.

Теоретически, если бы мы смогли построить действительную транзакцию, содержащую один вход типа `txin_to_script` или `txin_to_scripthash`, то образ ключа любого входа типа `txin_to_key`, который бы последовал за ним, был бы удалён. По сути, это бы стёрло информацию о том, что вход был потрачен в другой транзакции, о чём бы не смог узнать никто при попытке обработать блок.

Блок так или иначе был бы отклонён, поскольку содержал бы ошибочную транзакцию, но нас интересует только то, чтобы наша транзакция преодолела половину пути до базы данных и чтобы образы ключей уже потраченных входов были удалены.

Но наше ликование было недолгим, так как мы наткнулись на заграждение в функции `Blockchain::check_tx_inputs(...)`.

```
bool Blockchain::check_tx_inputs(...) const
{
  ...
  for (const auto& txin : tx.vin)
  {
    // make sure output being spent is of type txin_to_key, rather than
    // e.g. txin_gen, which is only used for miner transactions
    CHECK_AND_ASSERT_MES(txin.type() == typeid(txin_to_key), false, "wrong type id in tx input at Blockchain::check_tx_inputs");
    const txin_to_key& in_to_key = boost::get<txin_to_key>(txin);
    ...
  }
}
```

Любой вход транзакции (кроме майнингового входа) должен быть типа `txin_to_key`. Эта проверка происходит задолго до выполнения нашего ошибочного кода, поэтому у нас не остаётся шансов на использование ошибки.

## _Конец_

Надеемся, вам понравилось погружение в суть механизмов обработки блоков Monero и защиты от двойной траты. Даже несмотря на то, что данная ошибка, как выяснилось, не являлась уязвимостью, мы сообщили о ней, и она была исправлена.

И вот некоторые результаты, которыми бы нам хотелось поделиться:
- Даже хорошо проверенная кодовая база может содержать ошибки.
- Следует уделять особое внимание тем областям, которые были проверены в наименьшей степени.
- Необходимо разбираться в сути высокоуровневых механизмов и отслеживать возникающие вопросы по мере изучения.
- `CTRL + F` и `F12` (в VSCode) помогут вам в отслеживании функций.
- Следует проявлять здоровое любопытство и проверять допуски, которые вы делаете и которые имеются в коде.

---

_Источник: [Hunting for logic bugs in Monero](https://keyboardwarrior.be/blog/2021-03-24-monero-keyimage)_

_Перевод: [Mr. Pickles](https://t.me/v1docq47)_  
_Коррекция: [Kukima](https://t.me/Kukima)_