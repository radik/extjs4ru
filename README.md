Документация и руководства по Ext JS 4
======================================

[Markdown]: http://daringfireball.net/projects/markdown/
[Ext JS]: http://www.sencha.com/products/js/
[jsduck]: https://github.com/senchalabs/jsduck/
[Ext JS 4 RU]: https://github.com/radik/extjs4ru

Это перевод документации по [Ext JS 4][].

Все неравнодушные присоединяйтесь
---------------------------------

Для этого требуется:

1. Установить [jsduck][]
2. Клонировать проект - [Ext JS 4 RU][]
3. Отметить в STATUS.md какой раздел вы переводите, указав раздел, его статус (in progress) и ссылку на свой профиль в GitHub

<pre>
    guides/application_architecture (in progress)- <a href="http://github.com/radik">Radik</a>
</pre>

4. Перевести (переводить надо markdown-файлы с расширением *.md)
5. Скомпилировать

<pre>
    jsduck --guides sources/guides.json --output docs/
</pre>

6. Удостовериться, что все в порядке, поменять статус раздела в STATUS.md на 'done'

<pre>
    guides/application_architecture (done)- <a href="http://github.com/radik">Radik</a>
</pre>

7. Послать pull-request
8. Попасть в список 'contributors'

* Contributors
