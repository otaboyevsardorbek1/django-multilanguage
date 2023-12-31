

                              Multi Language Django - Sardor Codes 

- Oddiy, modelda ega bo'lgan websayt yaratish.

- Quyidagi o'zgaruvchilar shu holda ekanligiga ishonch hosil qilish.
  LANGUAGE_CODE = "uz"   // Asosiy tilni kodi yoziladi
  TIME_ZONE = "UTC"
  USE_I18N = True
  USE_L10N = True 

- Biz websyatimizni qaysi tillarda bo'lishini xohlaymiz shuni kiritamiz. Bizni holatda bu:

   from django.utils.translation import gettext_lazy as _
   LANGUAGES = (
    ('uz', _('Uzbek')),
    ('en', _('English')),
    )

- Session middlewaredan keyin shu middlewareni qo'shib qo'yamiz:
    'django.middleware.locale.LocaleMiddleware',    

- Tarjima fayllarni saqlsh uchun "locale" nomli fayl yaratib uni settings faylimizga kiritamiz:
  LOCALE_PATHS = [
    BASE_DIR/'locale/',
   ]  

- Locale faylni ichida esa bizning holatimizda "uz" va "en" fayllarini ham yaratib qo'yishimiz kerak

- Tayanch url prefix qo'shish:  
   ...
   from django.conf.urls.i18n import i18n_patterns

   urlpatterns = i18n_patterns(
      ...
   )

- Viewlarimizda tarjima qilish uchun matn kiritamiz:
   from django.utils.translation import gettext as _ 

   ...
   text = _("This is a simple text")


- Templatemizda tarjimaqilish uchun text kiritamiz:
  {% load i18n %}

  {% trans 'Hello World' %}

-Tarjimalarni yaratish uchun:
    $ django-admin makemessages --all --ingore=env
   ularni tasdiqlsh uchun:
    $ djago-admin compilemessages  

- Bizda "locale/uz/LC_MESSAGES/" va "locale/en/LC_MESSAGES/" da "django.po" fayli paydo bo'ladi.
  Undagi so'zlarni to'g'ri tarjimaqilib chiqamiz:


      #: .\templates\index.html:25
      msgid " Hello world "
      msgstr "Salom dunyo"

- Modellarni tarjimaqilish, 
  1. $ pip install django-modeltranslation
  2. INSTALLED_APPS ga qo'shish
  3. translation.py faylini yaratish appimizni ichida
  4. Bu paket orqali translatsiya unstuni qo'shish:
     from modeltranslation.translator import TranslationOptions, register
     from .models import New

     @register(New)
     class NewsTranslationOptions(TranslationOptions):
           fields = ('title', 'body')
  5. $ python manage.py makemigrations
     $ python manage.py migrate
     $ django-admin compilemessages

     Paketni o'rnatishdan oldingi ustunlar:
      title, body , date

     Paketni o'rnatishdan keyingi ustunlar:
      title, title_uz, title_en,  body, body_uz, body_en,  date
  
- Tarjima qilinidaginan tillar ro'yxatini templateda chiqarib qo'yish:
      {% get_current_language as current_language %}
      {% get_available_languages as available_languages %}
      {% get_language_info_list for available_languages as langs %}
            <button class="btn badge bg-primary dropdown-toggle" type="button" data-bs-toggle="dropdown" aria-expanded="false">
            {{current_language}} 
            </button>
            <ul class="dropdown-menu">
            {% for i in langs %}
            <li><a class="dropdown-item" href="/{{i.code}}">{{i.name_local}}</a></li>
            {% endfor %}
