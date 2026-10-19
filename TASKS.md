# Fast-food — nimalarni to'g'irlash kerak


Tartib bilan, birinchi punktdan boshlab qil. Har bir qismdan keyin `python manage.py runserver` qilib tekshirib tur.

Eslatma: loyiha nomlarini o'zgartirdim. Endi `config` bu **loyiha** (settings, urls), `fastfood` esa **app** (models, views).
`burger` app endi yo'q. Shablonlar loyiha ichidagi `templates/` papkada.

---

## 1-qism. Git ni tozalash

**1. `.gitignore` yo'q**
Oxirgi commit nomi "add .gitignore", lekin fayl yo'q. Shu sababli git ga `__pycache__` va `db.sqlite3` tushib qolgan.
`.gitignore` yarat, ichiga `__pycache__/`, `*.pyc`, `db.sqlite3`, `venv/`, `.venv/`, `.idea/` yoz. Keyin:
```
git rm -r --cached .
git add .
```
va commit qil.

---

## 2-qism. Sayt ko'rinishi uchun

**2. `static` papka yo'q**
Shablonlarda `css/style.css`, `images/hero-bg.jpg`, `js/custom.js` ishlatilgan, lekin loyihada bu fayllar yo'q.
Shablon (Feane) dan `css`, `js`, `images` papkalarini olib, loyiha ichida `static/` papkaga joyla.
`settings.py` ga shuni qo'sh:
```python
STATICFILES_DIRS = [BASE_DIR / 'static']
```

**3. `base.html` buzilgan**
Hozir `base.html` da `<html>`, `<head>`, `<body>` yo'q, faqat har xil linklar ro'yxati.
Uni qaytadan yoz: `about.html` dan to'liq sahifani ol, menyu, footer va skriptlarni qoldir,
o'rtasiga `{% block content %}{% endblock %}` qo'y.

**4. `index.html` bo'sh**
Ichida faqat `{% extends 'base.html' %}` va `ddddddd` bor. Blokdan tashqaridagi matn chiqmaydi.
Shablondagi bosh sahifa kontentini `{% block content %}` ichiga joyla.

**5. `menu.html` Django ga moslanmagan**
Tepasida `{% load static %}` yo'q, yo'llar oddiy yozilgan (`href="css/style.css"`, `images/favicon.png`).
Hammasini `{% static 'css/style.css' %}` ko'rinishiga o'tkaz.

---

## 3-qism. Sahifalar

**6. View va url lar yo'q**
Faqat `index` bor. `about.html`, `menu.html`, `book.html` uchun `views.py` da funksiya yoz
va `fastfood/urls.py` ga `path(...)` qo'sh. Har biriga `name=` ber.

**7. Menyu linklari noto'g'ri**
Menyuda `href="{% static 'about.html' %}"` deb yozilgan — bu `/static/about.html` ga olib boradi, 404 chiqadi.
Sahifalarga link `{% url 'about' %}` orqali qilinadi. Hamma shablonlarda to'g'irla.

**8. Hamma shablonlar `base.html` dan foydalansin**
`about.html`, `menu.html`, `book.html` — har biri to'liq sahifa nusxasi.
Har birida `{% extends 'base.html' %}` qil, menyu, footer va skriptlarni o'chir,
faqat o'rtadagi qismni `{% block content %}` ichida qoldir.

---

## 4-qism. Model va admin

**9. `models.py` bo'sh**
Menyu uchun `Food` modeli yarat: `title`, `description`, `price` (`DecimalField`), `image` (`ImageField`), `category`.
`__str__` ni unutma. `pip install pillow`, keyin `makemigrations` va `migrate` qil.

**10.** `admin.py` da `Food` ni ro'yxatdan o'tkaz. Rasmlar uchun `settings.py` ga `MEDIA_URL` va `MEDIA_ROOT` qo'sh,
`config/urls.py` da media fayllarni ulab qo'y.

**11.** `menu.html` dagi qotirib yozilgan taomlarni o'chir, `{% for food in foods %}` bilan bazadan chiqar.

**12.** `book.html` dagi formaga `method="post"` va `{% csrf_token %}` qo'sh.

---

## 5-qism. Kodni tozalash

**13.** `templates/href.http` — PyCharm ning namuna fayli, o'chir.

**14.** `fastfood/urls.py` da `from .views import *` emas, `from . import views` yoz, `views.index` deb ishlat.

**15.** `README.md` UTF-16 kodirovkada saqlangan, GitHub da buzilib ko'rinadi. UTF-8 da qaytadan saqla
va loyihani qanday ishga tushirishni yoz.

**16.** `base.html` dagi Google Maps (`key=YOUR_API_KEY`) ishlamaydi — kerak bo'lmasa o'chir.

---

Hammasini qilib bo'lgach, menga yoz — tekshirib beraman.
