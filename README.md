# Линтеры / Code Style

## Введение

Цель использования линтеров и статических анализаторов - упрощение восприятия и поддержки нашего кода (в т.ч. другими разработчиками) и отлавливание простых багов ещё на стадии написания кода.

Помимо использования линтеров также стоит, по возможности, придерживаться следующих принципов:

- Стараться использовать последние версии IDE, т.к. с каждой новой версией в них улучшаются возможности статического анализа и добавляются новые стандарты. Эта статья написана для phpstorm 2019 года. Если у вас более ранний шторм, можете, например, скачать с моего компа по samba. Для этого, в наутилусе нажмите ctrl+L и наберите следующий адрес

        smb://192.168.1.151/share/phpstorm

- Начинать новые проекты на последних стабильных релизах софта (перед этим убедиться, не будет ли проблем с развёртыванием). На текущий момент, это symfony5 и php7.4.1,
- Использовать плагины для фреймворка в IDE. Для симфони и ларавеля есть соответствующие плагины.
- Следить за актуальностью пакетов и за валидностью вашего composer.json, лучше включить синхронизацию IDE с composer.json и периодически делать composer update (отдельным коммитом с пометкой). Частые ошибки: указание минимальной версии php ниже, чем по факту применяется, отсутствие в json-е информации о необходимых php-extensions, блокировка подключаемых пакетов на определённой версии.
- Если вышел новый релиз фреймворка в рамках одной ветки, стоит обсудить с пмом и клиентом обновление до новой версии (на всякий случай, в отдельной ветке)

## **Вариант использования**

- Ставим sonar lint плагин для шторма (File→Settings→Plugins→Marketplace), отключаем бесполезную инспекцию literal to constant,
- Настраиваем phpcs для работы, как psr12 инспекция шторма (weak warning):
    - Установка в систему

            composer global require "squizlabs/php_codesniffer=*"

    - Настраиваем в шторме: 
    Сначала подключаем тулзу: Settings→Languages & Frameworks→PHP→Quality Tools→PHP_CodeSniffer. Указываем путь /home/{user}/.composer/vendor/bin/phpcs
    После настраиваем инспекцию: Settings→Editor→Inspections→PHP→Quality Tools→PHP CS Fixer validation.
    - фиксер из этого же пакета не ставим, вместо этого...
- Для рефакторинга применяем php-cs-fixer с кастомными правилами
    - Установка в систему

            composer global require friendsofphp/php-cs-fixer

    - Настройка правил
    Качаем наш код-стайл - [https://github.com/timofey-n/zimalab-coding-style](https://github.com/timofey-n/zimalab-coding-style)
    - Настройка в шторме (необязательно, если применяем standalone фикс): Подключаем тулзу по аналогии с PHP_CS. Подключаем инспекцию, указываем "Custom" ruleset, указываем путь до скаченного php_cs файла с code style.
    - Применение фиксера для фикса всего проекта (standalone fix):
    Нужно убедиться, что все изменения закоммитаны. Выполняем команду в корне проекта:

            {/path/to/}php-cs-fixer fix --config={/absolute/path/to/}php-cs-fixer-zimalab-coding-style.php_cs

        После всё проверяем и коммитаем.

    - Применение фикса на коммит (пока с этим проблемы, т.к. шторм применяет все доступные инспекции, не только наши. Как разберёмся - пункт дополнится)
    - Статический анализ всего проекта штормом (sonarlint+phpcs+php-cs-fixer+phpstorm)
    Это тоже очень полезно делать. Делается тут: Code→Inspect Code→Whole project. Кэш и прочие левые папки должны быть помечены, как **Excluded**.