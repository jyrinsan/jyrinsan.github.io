[Etusivu](index.html) 
&emsp;[PW1](pw1.html)
&emsp;PW2
&emsp;[PW3](pw3.html)
&emsp;[PW4](pw4.html)
&emsp;[PW5](pw5.html)
&emsp;[PW6](pw6.html)

# Harjoitus 2 - ListView

```
Nimi              Sanna Jyrinki
Oppilaitos        Haaga-Helian ammattikorkeakoulu
Kurssi            Python weppipalvelu - ideasta tuotantoon ICT8TN034-3002
Opettaja          Tero Karvinen
Tietokoneena      AMD Ryzen 5 PRO 4650U with Radeon Graphics 2.10 GHz
Käyttöjärjestelmä Windows 11 Pro, Versio 21H2
Linux             Oracle Virtual Box 6.1, Debian 11.3
```

## Lähteet

Django Contributors. n.a. Model field reference. Luettavissa [https://docs.djangoproject.com/en/3.2/ref/models/fields/#field-types](https://docs.djangoproject.com/en/3.2/ref/models/fields/#field-types). Luettu 24.5.2022. 

Karvinen, T. 2022a. Python Web Service From Idea to Production. Luettavissa [https://terokarvinen.com/2021/python-web-service-from-idea-to-production-2022/](https://terokarvinen.com/2021/python-web-service-from-idea-to-production-2022/). Luettu 23.5.2022.

Karvinen, T. 2022b. Django 4 Instant Customer Database Tutorial. Luettavissa [https://terokarvinen.com/2022/django-instant-crm-tutorial/](https://terokarvinen.com/2022/django-instant-crm-tutorial/). Luettu 23.5.2022.

MDN Contributors. n.a. Django Tutorial Part 3: Using models. Luettavissa [https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django/Models](https://developer.mozilla.org/en-US/docs/Learn/Server-side/Django/Models). Luettu 24.5.2022.

## a) Uusi django-projekti

Tehtävänanto löytyy Tero Karvisen kurssimateriaalista (Karvinen, 2022a).

### django-asennus
Asensin django ympäristön samaan tapaan kuin edellisessä tehtävässä [pw1](pw1.html).

Loin kotihakemistossani hakemiston uudelle projektille `bookstore`
```
cd
mkdir bookstore
cd bookstore
```

Loin env-virtuaaliympäristön
```
virtualenv --system-site-packages -p python3 env
source env/bin/activate
which pip # tarkastetaan, että polku env:n sisällä
```

Asennetaan django 
```
micro requirements.txt # sisällöksi django==3.2
pip install -r requirements.txt
django-admin --version # tarkistetaan asennus
```

Perustetaan projekti, ajetaan migraatiot
```
django-admin startproject jyrinkicom
cd jyrinki.com
./manage.py makemigrations
./manage.py migrate
./manage.py runserver 
pwgen -s 20 1
/manage.py createsuperuser
# # testaus http://127.0.0.1:8000/admin
```

Testaus selaimella 
- http://127.0.0.1:8000
<kbd><img src="pw2_images/pw2_img1.PNG" /></kbd>
- http://127.0.0.1:8000/admin
<kbd><img src="pw2_images/pw2_img2.PNG" /></kbd>

### bookstore sovellus
Luon `bookstore` sovelluksen
```
./manage.py startapp bookstore
micro jyrinkicom/settings.py # lisätään sovellus INSTALLED_APPS kohtaan
```

Luodaan model `micro bookstore/models.py`
```
from django.db import models

class Book(models.Model):
	name = models.CharField(max_length=300)
	author = models.CharField(max_length=300)
	pubYear = models.IntegerField()

	def __str__(self):
		return self.name
```

Tehdään migraatiot
```
./manage.py makemigrations
./manage.py migrate
```

Rekisteröidään tietokanta `micro bookstore/admin.py`
```
from django.contrib import admin
from . import models

admin.site.register(models.Book)
```

Testataan lisätä pari kirjaa admin-käyttöliittymällä
<kbd><img src="pw2_images/pw2_img3.PNG" /></kbd>

### webbisivu 
Muokataan `micro jyrinki.com/urls.py`
```
from django.contrib import admin
from django.urls import path
from bookstore import views

urlpatterns = [
    path('admin/', admin.site.urls),
    path('books/', views.BookListView.as_view())
]
```

Muokataan `micro bookstore/views.py`
```
from django.views.generic import ListView
from . import models

class BookListView(ListView):
	model = models.Book
```

Tehdään hakemisto html-templatelle
```
mkdir bookstore/templates
mkdir bookstore/templates/bookstore
```

Restartataan kehitysserveri `./managepy runserver`

Muokataan `micro bookstore/templates/bookstore/book_list.html`
```

```