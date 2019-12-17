PASSOS BÁSICOS DJANGO
==================================================

[VENV]  
[Instalar Django]  
[Iniciar projeto]  
[Alterar configurações de settings.py]  
[Testar o servidor]  
[URLS]  
[Views]  
[Models]  
[Django Admin]  
[Templates]  
[Arquivos estáticos]  
[Arquivos de media]  

## VENV

Criar virtual enviroment:  
```shell
python3 -m venv <nome_do_projeto>
```
    
Ativar venv no linux:
    
```shell
source <nome_do_projeto>/bin/activate
```

ou

```shell
. <nome_do_projeto>/bin/activate>
```
    
Ativar venv no windows:
    
```shell
venv\Scripts\activate
```
        
## Instalar django:

```shell
pip install django
```

## Iniciar projeto:

```shell
django-admin startproject <nome do projeto> .
```

## Alterar configurações de settings.py:

```python
LANGUAGE_CODE = 'pt-br'
```

```python
TIME_ZONE = 'America/Sao_Paulo'
```

## Testar o servidor:

```shell
python manage.py runserver
```

## URLS:
    
```python
path('url/<argumento>/', <função ou view>)
```

**Url:** é o destino a ser digitado no navegador  
**Argumento:** no formato <tipo_variável : nome_variável>  
**Função ou view:** sera importada do arquivo view e inserida como argumento de path

É possível extrair urls de uma app, basta incluir a classe include e seu arquivo urls.py da aplicação no arquivo urls.py do seu projeto:
    
```python
from django.urls import include
from <app> import urls as <app>_urls
```
        
E criar um path que levará para as urls da aplicação:
    
```python
path('exemplo/',include(<app>_urls))
```

Para o funcionamento das urls é necessário importar:

```python
from django.contrib import admin
from django.urls import path
```

Podem ser passados argumentos para as views:

```python
path('hello/<str:nome> ', hello)
```
    
## Views:

Importar a classe HttpResponse:

```python
from django.http import HttpResponse
```
    
Http - Request e Response básico da view:

```python
def hello(Request):
    return HttpResponse('Hello World!')
```

Podem ser passados argumentos para as views:

```python
def hello(Request, nome):
    return HttpResponse('Hello ' + str(nome) + '!')
```
            
## Models:

Antes de criar um model, deve-se criar um app onde este model será utilizado.

```shel
python manage.py startapp <nome do app>
```

E registra-lo em settings.py -> INSTALLED_APPS. 

Depois disso no diretório do app terá o arquivo models.py.

Neste arquivo importe a classe models:

```python
from django.db import models
```

Definindo classe Person:

```python
#definindo a classe(Tabela) e os fields(Atributos)
class Person(models.Model):
    first_name = models.CharField(max_length=30)
    last_name = models.CharField(max_length=30)
    age = models.IntegerField()
    salary = models.DecimalField(max_digits=5,decimal_places=2)
    bio = models.TextField()

#função para quando listar os itens do banco na página de admin, retornar o nome e idade
def __str__(self):
    return self.first_name + " " + str(self.age)
```

Cada classe em models vai para migrate.
Estas futuramente serão as tabelas do banco de dados que foi definido em settings.py -> DATABASES.

Aplicar as migrações:

```shell
#aplica os models no BD
python manage.py migrate
```

```shell
#cria o migrate em migrations
python manage.py makemigrations
```

Toda vez que realizar alterações em um model, deve ser aplicado makemigrations.
    
## Django Admin:
    
Django admin serve para gerenciamento do backend.

Criando um super usuário:

```shell
python manage.py createsuperuser
```

Para exibir opções de adição, exclusão ou edição de dados dos models na página de admin, é necessário importar o model para o arquivo admin.py e registrar-lo:

```python
from .models import <nome do model>

admin.site.register(<nome do model>)
```

## Templates:
    
Criar o  diretório na raiz do projeto.

Adicionar o nome do diretório de templates em settings.py -> TEMPLATES -> 'DIRS'

```python
#Importar a classe render para o arquivo views.py
from django.shortcuts import render

#Função que retorna o render do template 
def hello(request):
    return render(request, 'index.html')
```

Retornar variáveis como argumento de render:

v_idade recebe idade e é retornada como argumento.

```python
return render(request, 'pessoa.html', {'v_idade':idade})
```

No arquivo html, basta adicionar a variável entre  {{}} - chaves duplas.

Por exemplo:

```html
<html>
    <body>
        A idade é: {{ v_idade }}
    <\body>
<\html>
```

No arquivo .html também é possível utilizar Jinja. Exemplo:

```python
{% if v_idade > 0 %}
    A pessoa tem {{ v_idade }} anos.
{% else %}
    Pessoa não encontrada
{% endif %}
``` 
    
## Arquivos estáticos:

Adicionar o seguinte código em settings.py:

```python
STATICFILES_DIRS = ['<nome que quiser>']
```

Criar o diretório na raiz do projeto e adicionar os arquivos estáticos nele.

Carregue os arquivos estáticos nos templates ou qualquer html:

```python
#Tem que ser a primeira linha do código
{% load static %}
```

Adicione o link para o arquivo estático no head do arquivo html:

```html
<link rel="stylesheet" href="{% static 'style.css' %}">
```
        
## Arquivos de media:

```python
#Adicionar o campo MEDIA_URL no arquivo settings.py, nela adicionar a variável de media.
#Começa e termina com barras.
MEDIA_URL = '/media/'

#Adicinar o campo MEDIA_ROOT, nele adicionar o nome do diretório onde ficarão os arquivos de mídia.
MEDIA_ROOT = '<o nome que quiser>'
```
Criar o diretório na raiz do projeto.

Na classe dentro de models deve conter o field para imagem passando como arqumento o nome do diretório:

```python
photo = models.ImageField(upload_to='fotos_produtos', null=True, blank=True)
```

Para conseguir exibir os arquivos de mídia será necessário adicionar algumas classes em urls.py:

```python
from django.conf import settings
from django.conf.urls.static import static

#E adicionar este trecho após as urlpatterns:

 + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)
 ```

 [VENV]: https://github.com/hemilioaraujo/DjangoNotes/blob/master/DjangoNotes.md#venv
 [Instalar Django]: https://github.com/hemilioaraujo/DjangoNotes/blob/master/DjangoNotes.md#instalar-django
 [Iniciar projeto]: https://github.com/hemilioaraujo/DjangoNotes/blob/master/DjangoNotes.md#iniciar-projeto
 [Alterar configurações de settings.py]: https://github.com/hemilioaraujo/DjangoNotes/blob/master/DjangoNotes.md#alterar-configura%C3%A7%C3%B5es-de-settingspy
 [Testar o servidor]: https://github.com/hemilioaraujo/DjangoNotes/blob/master/DjangoNotes.md#testar-o-servidor
 [URLS]: https://github.com/hemilioaraujo/DjangoNotes/blob/master/DjangoNotes.md#urls
 [Views]: https://github.com/hemilioaraujo/DjangoNotes/blob/master/DjangoNotes.md#views
 [Models]: https://github.com/hemilioaraujo/DjangoNotes/blob/master/DjangoNotes.md#models
 [Django Admin]: https://github.com/hemilioaraujo/DjangoNotes/blob/master/DjangoNotes.md#django-admin
 [Templates]: https://github.com/hemilioaraujo/DjangoNotes/blob/master/DjangoNotes.md#templates
 [Arquivos estáticos]: https://github.com/hemilioaraujo/DjangoNotes/blob/master/DjangoNotes.md#arquivos-est%C3%A1ticos
 [Arquivos de media]: https://github.com/hemilioaraujo/DjangoNotes/blob/master/DjangoNotes.md#arquivos-de-media
