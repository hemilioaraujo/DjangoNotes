PASSOS BÁSICOS DJANGO
==================================================

```python
s = "Python syntax highlighting"
print s
```


```shell
pip install django
```

### VENV

    * Criar virtual enviroment:
    
        ```python3 -m venv <nome_do_projeto>```
    
    * Ativar venv no linux:
    
        ```
        source <nome_do_projeto>/bin/activate
        ```
        
        ou
        
        ```. <nome_do_projeto>/bin/activate>```
    
    * Ativar venv no windows:
    
        ```> cd venv/Scripts```
        
        ```> activate```
        
### Instalar django:

    ```pip install django```

### Iniciar projeto:

    ```django-admin startproject <nome do projeto> .```

### Alterar configurações de settings.py:

    ```LANGUAGE_CODE = 'pt-br'```
    
    ```TIME_ZONE = 'America/Sao_Paulo'```

### Testar se o servidor irá rodar:

    ```python manage.py runserver```

### Criar urls:
    
    ```path('url/<argumento>/', <função ou view>)```
    
    > <b>Url</b> é o destino a ser digitado no navegador
    >
    > <b>Argumento</b> no formato <tipo_variável : nome_variável>
    >
    > <b>Função ou view</b> sera importada do arquivo view e inserida como argumento de path

    É possível extrair urls de uma app, basta incluir a classe include e seu arquivo urls.py da aplicação no arquivo urls.py do seu projeto:
    
        ```from django.urls import include```<\ br>
        ```from <app> import urls as <app>_urls```
        
    E criar um path que levará para as urls da aplicação:
    
        ```path('exemplo/',include(<app>_urls))```

    Necessário importar:
    
        ```from django.contrib import admin```
    
        ```from django.urls import path```

    Podem ser passados argumentos para as views:

    	```path('hello/<str:nome> ', hello)```
    
### Views:
    
    ```from django.http import HttpResponse```
    
    Http - Request e Response básico da view:
        
        ```def hello(Request):
                return HttpResponse('Hello World!')```

    Podem ser passados argumentos para as views:

    	```def hello(Request, nome):
                return HttpResponse('Hello ' + str(nome) + '!')```
            
### Models:

    Antes de criar um model, deve-se criar um app onde este model será utilizado.
    
    ```python manage.py startapp <nome do app>```
    
    E registra-lo em settings.py -> INSTALLED_APPS. 
    
    Depois disso no diretório do app terá o arquivo models.py.
    
    Neste arquivo importe a classe models:
    
    ```from django.db import models```
    
    Definindo classe Person:
    
    ```class Person(models.Model):```
    
    Definindo os fields:
    
    ```first_name = models.CharField(max_length=30)```
    ```last_name = models.CharField(max_length=30)```
    ```age = models.IntegerField()```
    ```salary = models.DecimalField(max_digits=5,decimal_places=2)```
    ```bio = models.TextField()```
    
    Cada classe em models vai para migrate.
    Estas futuramente serão as tabelas do banco de dados que foi definido em settings.py -> DATABASES.
    
    Aplicar as migrações:
    
    ```python manage.py migrate #aplica os models no BD```<br>
    ```python manage.py makemigrations #cria o migrate em migrations```
    
    Toda vez que realizar alterações em um model, teve ser aplicado makemigrations.

    Para ser exibido os nomes das classes corretamente na área de admin, deve ser criada uma função na classe desejada para retonar o nome ou o que for necessário retornar:

    	'''def __str__(self):'''
        	'''return self.nomeProduto + " " + str(self.preco)'''
    
    
### Django Admin:
    
    Django admin serve para gerenciamento do backend.
    
    Criando um super usuário:
        
        ```python manage.py createsuperuser```
        
    Para exibir opções de adição, exclusão ou edição de dados dos models na página de admin, é necessário importar o model para o arquivo admin.py e registrar-lo:
    
    ```from .models import <nome do model>```<br>
    
    ```admin.site.register(<nome do model>)```

### Templates:
    
    Criar o  diretório no diretório raiz do projeto.
    
    Adicionar o nome do diretório de templates em settings.py -> TEMPLATES -> 'DIRS'
    
    Importar a classe render para o arquivo views.py:
    
        ```from django.shortcuts import render```
        
    Retornar com render:
    
        ```return render(request, 'index.html')```
        
    Retornar variáveis como argumento de render:
    
        v_idade recebe idade e retornada como argumento.
    
        ```return render(request, 'pessoa.html', {'v_idade':idade})```
    
    No arquivo html, basta adicionar a variável entre  {{}} - chaves duplas. Por exemplo:
    
    ```<html>```<br>
    ```<body>```<br>
    ```     A idade é: {{ v_idade }}```<br> 
    ```<\body>```<br>
    ```<\html>```<br>
    
    No arquivo .html também é possível utilizar Jinja. Exemplo:
    
    ```{% if v_idade > 0 %}```<br> 
    ```    A pessoa tem {{ v_idade }} anos.```<br> 
    ```{% else %}```<br> 
    ```     Pessoa não encontrada```<br> 
    ```{% endif %}```<br> 
    
### Arquivos estáticos:
    
    Adicionar a lista STATICFILES_DIRS no arquivo settings.py, nela adicionar os nomes dos diretórios de arquivos estáticos.
    
    Criar o diretório no diretório raiz do projeto e adicionar os arquivos estáticos nele.
    
    Carregue os arquivos estáticos nos templates ou qualquer html:
        
        ```{% load static %}```
        
### Arquivos de media:

    Adicionar o campo MEDIA_URL no arquivo settings.py, nela adicionar a variável de media.
    
    ```MEDIA_URL = '/media/'```
    
    Adicinar o campo MEDIA_ROOT, nele adicionar o nome do diretório onde ficarão os arquivos de mídia.
    
    ```MEDIA_ROOT = 'media'```
    
    Criar o diretório no diretório raiz do projeto.
    
    Para conseguir exibir os arquivos de mídia será necessário adicionar algumas classes em urls.py:
    
    ```from django.conf import settings```<br>
    ```from django.conf.urls.static import static```
    
    E adicionar este trecho após as urlpatterns:
    
    ``` + static(settings.MEDIA_URL, document_root=settings.MEDIA_ROOT)```
    
