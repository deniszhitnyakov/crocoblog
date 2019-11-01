---
title: "Настройка лендинга"
date: 2019-10-25T16:41:05+03:00
draft: false
---
## 1. Добавление небходимых скриптов
1.1 Перед закрывающим `</body>` добавить:

{{< highlight html >}}
<!-- НЕОБХОДИМЫЕ СКРИПТЫ -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.4.1/jquery.min.js" integrity="sha256-CSXorXvZcTkaix6Yvo6HppcZGetbYMGWSFlBw8HfCJo=" crossorigin="anonymous"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/js-url/2.5.3/url.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/js-cookie/2.2.1/js.cookie.min.js"></script>
{{< / highlight >}}

1.2 После этого добавить:

{{< highlight html >}}
<!-- ЗАПИСЬ ПИКСЕЛЯ В COOKIES -->
<script>
	$(function() {
		if (url('?pixel')) Cookies.set('pixel', url('?pixel'), {expires: 30});
	});
</script>
{{< / highlight >}}

## 2. Получение IP посетителя

После необходимых скриптов добавить:

{{< highlight html >}}
<!-- ПОЛУЧЕНИЕ IP ПОСЕТИТЕЛЯ -->
<script>
    $.getJSON("http://gd.geobytes.com/GetCityDetails?callback=?",function(t){window.ip=t.geobytesremoteip});
</script>
{{< / highlight >}}

## 3. Добавить обработчик формы

Everad

{{< highlight html >}}
<!-- ИНТЕГРАЦИЯ С ПП -->
<script>
    $(function() {
    $('body').on('submit', 'form', function(e) {
        e.preventDefault();

        form = this;
        button = $(this).find('button[type="submit"]');
        buttonText = $(button).text();

        $(button).text('Обработка...');

        data = {
            fullName: $(this).find('input[name="name"]').val(),
            campaign_id: 906780,
            ip: window.ip,
            phone: $(this).find('input[name="phone"]').val(),
            country_code: 'RU',
            click_id: Cookies.get('click_id')
        };

        if (!data.fullName) {
            alert('Заполните ФИО!');
            return;
        }

        if (!data.phone) {
            alert('Введите телефон!');
            return;
        }

        console.log(data);

        $.ajax('https://beta.dolphin.ru.com/everad.php', {
            method: 'post',
            data: data,
            success: function(r) {
                $(button).text(buttonText);
                location.href = '../thanks.html';
            }
        });
    });
});
</script>
{{< / highlight >}}

M4Leads

{{< highlight html >}}
<!-- ИНТЕГРАЦИЯ С ПП -->
<script>
	$(function() {
    $('body').on('submit', 'form', function(e) {
        e.preventDefault();

        form = this;
        button = $(this).find('button[type="submit"]');
        buttonText = $(button).text();

        $(button).text('Обработка...');

        data = {
            fullName: $(this).find('input[name="name"]').val(),
            offerId: 322,
            phone: $(this).find('input[name="phone"]').val(),
            partnerId: 293895,
            'access-token': '71c437d5bf75a76ff5d89c24b567f047',
            country: 'KZ',
            price: 0,
            sub_id: ['dolphin', Cookies.get('click_id')]
        };

        if (!data.fullName) {
            alert('Заполните ФИО!');
            return;
        }

        if (!data.phone) {
            alert('Введите телефон!');
            return;
        }

        console.log(data);

        $.ajax('https://api.m4leads.com/order/add', {
            method: 'get',
            data: data,
            success: function(r) {
                $(button).text(buttonText);
                location.href = '../thanks.html';
            }
        })
    });
});
</script>
{{< / highlight >}}