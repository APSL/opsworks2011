
# Goldcar Amazon Opsworks


Creantbits, Octubre 2014

.notes: Pressing 2 will display these fascinating notes

.fx: titleslide

---

<img src="img/aws_panel.png" />


.fx: imageslide 


---

# AWS OpsWorks

* *DevOps Application Management Service*

---

# AWS OpsWorks

* *DevOps Application Management Service*
* Agrupación y orquestación de servicios AWS
* Orientado al ciclo de vida de la **Aplicación**

---

## ¿Porqué OpsWorks? - Simplicidad vs Control

<img src="img/aws_control.png" />

* Heroku y OpenShift estarían cerca de OpsWorks


---

## Características OpsWorks

* Altamente customizable. Al final, todo acaba en una ejecución Chef.
* Arquitectura repetible. Orientada a fallar.
* Sólo pagas por recursos AWS usados.
* Arquitectura auto documentada: Modelado Layers, recetas mantenidas en Git.
* Distintos entornos idénticos y repetibles: prod, pre, test
* Mejor con arquitecturas "shared noting" (12factor.net)

---

## ¿Qué provee OpsWorks?

* Provisionamiento instancias. Integración AWS
* deploy + rollback. Automatización del ciclo de vida de una aplicación
* Gestión de la configuración
* Balanceo carga
* Escalado automático. Auto Healing
* Logs + Monitorización
* Consola web, aws-cli, API (python boto)
* Permisos


---

# Conceptos OpsWorks fundamentales

* Stack
* Layers
* Aplicaciones
* Instancias

Nomenclatura propia OpsWorks.

---

<img src="img/welcome.png" />


.fx: imageslide 


---
# Stack

<img class="float-right" src="img/stack.png" />

Es el contenedor de más alto nivel, y abarca una o más aplicaciones y todos sus
recursos necesarios. Para una aplicación completa, crearemos un stack, pero
podremos usar varios stacks para entornos de producción, test o pre-produción.


---
# Layer

<img class="float-right" src="img/layer.png" />

* Cada stack tiene uno o más layers. 
* Podemos definir cada layer como distintos roles de servicio. 
* Por ejemplo, podríamos tener un layer de balanceo de carga, uno de memcached, y otro de servidor de aplicaciones. 
* Podremos usar los layers que vienen de serie, o Custom Layers


.fx: align-right

---

# Aplicación

<img class="float-right" src="img/app.png" />

* Es el código de la aplicación. 
* La fuente puede ser S3, git o un zip. En nuestro caso, usamos repositorios git. 
* La aplicación se despliega en un layer. 
* Un layer puede contener distintas aplicaciones que se escalarán y configurarán conjuntamente.

---

# Instancias

<img class="float-right" src="img/instances.png" />

* Las instancias se asignan a uno o más layers. 
* Por ejemplo, podemos asignar instancias al layer de haproxy.
* Podemos iniciar instancias **manualmente**, **por tiempo**, o **por carga** bajo demanda (memoria, carga o uso de cpu).

---

<img  src="img/3stack.png" />

.fx: imageslide

---


<img  src="img/3stack-layers.png" />

.fx: imageslide


---


<img  src="img/3stack-layers-instances.png" />

.fx: imageslide

---

# Arquitectura OpsWorks

<img  src="img/agent.png" />

* OpsWorks backend genera eventos: deploy app, escalado.
* Opsworks agent  escucha eventos del ciclo de vida
* El agente lanza *Chef Solo*, sobre datos json

---

<img  src="img/chef-cmp.png" />

* Versiones soportadas: 11.4 o 11.10
* Lanzado por los eventos del ciclo de vida
* Cada evento viene con un estado JSON

---

# Built-in Layers

* Haproxy
* Rails
* PHP
* Node.js
* Java
* Mysql
* Memcached

---

# Eventos del ciclo de vida

<img src="img/cicle.png" />

Las recetas se definen en cada **layer**, y se ejecutan por eventos generados por el ciclo 
de vida de la aplicación.

---

<img src="img/php-recipes.png" />

.fx: imageslide

---

# DEMO

---

# Opciones Customización

* Sobreescribir atributos en "Custom Json"
* Sobreescribir atributos chef vía receta custom
* Sobreescribir Plantilla Chef
* Deploy hooks
* Proveer receta customizada en Layer por defecto
* Proveer receta customizada en Layer Custom.

(Ordenados de más simples a más control)

---

# Deploy hooks:

    !bash
    $ cd mi_app_code
    $ ls deploy/
        before_migrate.rb
        before_symlink.rb
        before_restart.rb
        after_restart.rb

    $ cat deploy/after_restart.rb

    Chef::Log.info("GC: Ejecutando before_symlink hook..")
    node[:deploy].each do |application, deploy|
        app_root = "#{deploy[:deploy_to]}/current"
        execute "chmod -R g+rw #{app_root}/cache" do
        end
    end

---

# ¿Preguntas?

---

# Gracias



