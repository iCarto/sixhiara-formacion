# SIXHIARA. Formación para ITs

Documentación y apoyos para la formación al equipo IT de las ARAs

## Puesta en marcha

## Editando la documentación
La documentación del curso está hecha con [mkdocs](https://www.mkdocs.org/)

Para instalar `mkdocs` se recomienda usar un [`virtualenv`](https://virtualenvwrapper.readthedocs.io/en/latest/)

```
$ git clone git@gitlab.com:icarto/sixhiara-formacion.git
$ mkvirtualenv -a sixhiara-formacion sixhiara-formacion
$ workon sixhiara-formacion
$ pip install -r requirements.txt
$ mkdocs serve

INFO    -  Building documentation... 
INFO    -  Cleaning site directory 
[I 180903 17:55:33 server:292] Serving on http://127.0.0.1:8000
[I 180903 17:55:33 handlers:59] Start watching changes
[I 180903 17:55:33 handlers:61] Start detecting changes

```

Con cualquier editor de markdown se puede ir actualizando la documentación.
