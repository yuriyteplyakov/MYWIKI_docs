<!-- Вторичная кнопка - контрастная с изменением цвета -->
                <a href="#" class="border-2 border-white hover:border-secondary bg-white/10 backdrop-blur-sm hover:bg-secondary text-white hover:text-white px-8 py-4 rounded-full transition-all duration-300 font-medium text-lg">
                    Как создаются
                </a>



@BotFather

https://t.me/PayhoneyBot

Use this token to access the HTTP API:
8053515067:AAETisP8V-bsPG5W5WxYbEWydqbh4qzBOec

- проверка chat_id
https://api.telegram.org/bot8053515067:AAETisP8V-bsPG5W5WxYbEWydqbh4qzBOec/getUpdates

chat_id 424869608

python manage.py set_tg_webhook
- для прода 
python manage.py set_tg_webhook

для кнопки

<a href="{% url 'order:buy_via_telegram' product.id %}?qty=1"
   class="inline-block bg-secondary hover:bg-secondary/90 text-white px-6 py-3 rounded-full transition">
  Купить через Telegram
</a>

Если товара нет на складе и он не made_to_order — кнопку выключайте (disabled). Если made_to_order=True, текст меняйте на «Заказать через Telegram» — логика в шаблоне остаётся прежней.

- для прода
DEBUG = False
ALLOWED_HOSTS = ['example.com']
CSRF_COOKIE_SECURE = True
SESSION_COOKIE_SECURE = True
SECURE_SSL_REDIRECT = True
SECURE_HSTS_SECONDS = 31536000
SECURE_HSTS_INCLUDE_SUBDOMAINS = True
SECURE_HSTS_PRELOAD = True
SECURE_REFERRER_POLICY = "same-origin"





<a href="{% url 'order:buy_via_telegram' product.id %}?qty=1"
   class="w-full bg-secondary hover:bg-secondary/90 text-white px-6 py-3 rounded-full text-center block transition-all">
  Купить через Telegram
</a>
можно добавить кнопку для покупки одного товара минуя корзину


Настройте административную панель для сайта

python manage.py shell

в shell

from django.contrib.sites.models import Site
site = Site.objects.get(id=1)
site.domain = 'yourdomain.com'  # замените на ваш домен
site.name = 'Your Site Name'    # замените на название сайта
site.save()


Проверьте работу sitemap
Перейдите по адресу /sitemap.xml на вашем сайте, чтобы убедиться, что sitemap работает правильно.

Добавьте ссылку на sitemap в robots.txt
Создайте или обновите файл robots.txt в корне вашего статического содержимого:

User-agent: *
Allow: /

Sitemap: https://yourdomain.com/sitemap.xml

Отправьте sitemap в поисковые системы
После деплоя сайта отправьте sitemap в Google Search Console и Яндекс.Вебмастер.

Дополнительные настройки (опционально)
Если у вас много товаров, можно добавить кеширование и разбивку на части:

# catalog/sitemaps.py
from django.contrib.sitemaps import Sitemap
from django.core.cache import cache

class CachedProductSitemap(ProductSitemap):
    def items(self):
        # Кешируем запрос на 1 час
        cache_key = 'sitemap_products'
        products = cache.get(cache_key)
        if not products:
            products = list(Product.objects.filter(available=True))
            cache.set(cache_key, products, 3600)
        return products

И обновите urls.py:

sitemaps = {
    'products': CachedProductSitemap,
    # ... остальные sitemaps
}