---
title: "Настройка прокладки"
date: 2019-10-25T16:41:01+03:00
draft: false
---

## 1. Добавление небходимых скриптов
1.1 Перед закрывающим `</body>` добавить:

{{< highlight html >}}
<!-- НЕОБХОДИМЫЕ СКРИПТЫ -->
<script>
	if(typeof jQuery=='undefined')
	{
		var headTag = document.getElementsByTagName("head")[0];
		var jqTag = document.createElement('script');
		jqTag.type = 'text/javascript';
		jqTag.src = 'https://cdnjs.cloudflare.com/ajax/libs/jquery/3.4.1/jquery.min.js';
		headTag.appendChild(jqTag);
	}
</script>
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


## 2. Добавление скрипта трекера

После необходимых скриптов добавить:

{{< highlight html >}}
<!-- DOLPHIN ТРЕКЕР -->
<script src="https://beta.dolphin.ru.com/js/tracker.min.js"></script>
{{< / highlight >}}

## 3. Получение IP посетителя

После скрипта трекера добавить:

{{< highlight html >}}
<!-- ПОЛУЧЕНИЕ IP ПОСЕТИТЕЛЯ -->
<script>
    $.getJSON("http://gd.geobytes.com/GetCityDetails?callback=?",function(t){window.ip=t.geobytesremoteip});
</script>
{{< / highlight >}}

## 4. Добавление формы заказа

4.1 Добавить блоки формы на странице

Добавить форму можно сколько угодно раз. Для этого в нужных местах страницы нужно добавить один и тот же код:

{{< highlight html >}}
<div class="prelander-form"></div>
{{< / highlight >}}

4.2 Добавить скрипт создания формы

После скрипта получения IP адреса добавить:

{{< highlight html >}}
<!-- СОЗДАНИЕ ФОРМ -->
<script>
    $(function () {
        $.ajax('https://beta.dolphin.ru.com/widgets/prelander-form.php', {
            method: 'post',
            data: {
                offer_name: 'НАЗВАНИЕ ОФФЕРА',
                price: 196,
                currency: 'руб'
            },
            success: function(formHtml) {
                $('div.prelander-form').each(function(i, div) {
                    $(div).html( formHtml );
                });
                $.getScript('https://beta.dolphin.ru.com/widgets/prelander-form-countdown.js');
                $('head').append('<link rel="stylesheet" href="https://beta.dolphin.ru.com/widgets/prelander-form.css" type="text/css" />');
            }
        })
    });
</script>
{{< / highlight >}}

## 5. Добавить обработчик формы

Everad

{{< highlight html >}}
<!-- ИНТЕГРАЦИЯ С ПП -->
<script>
    $(function () {
        $('form').submit(function (e) {
			
			form = this;
			e.preventDefault();
			
			button = $(this).find('button[type="submit"]');
			buttonText = $(button).text();
			$(button).text('Обработка...');
			
            data = {
                fullName: $(this).find('input[name="name"]').val(),
                campaign_id: 906780	,
                ip: window.ip,
                phone: $(this).find('input[name="phone"]').val(),
                country_code: 'RU',
                click_id: Cookies.get('click_id')
			};
			
			console.log(data);
			
            $.ajax('https://beta.dolphin.ru.com/everad.php', {
                method: 'post',
                data: data,
                success: function (r) {
                    $(button).text(buttonText);
                    location.href = 'thanks.html';
                }
            })
        });
    })
</script>
{{< / highlight >}}

M4Leads

{{< highlight html >}}
<!-- ИНТЕГРАЦИЯ С ПП -->
<script>
	$(function () {
		$('form').submit(function (e) {
			form = this;
			e.preventDefault();

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

			console.log(data);
			
			$.ajax('https://api.m4leads.com/order/add', {
				method: 'get',
				data: data,
				success: function (r) {
					$(button).text(buttonText);
					location.href = 'thanks.html';
				}
			})
		});
	})
</script>
{{< / highlight >}}