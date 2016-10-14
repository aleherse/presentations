
# **INTRODUCCIÓN Al FRAMEWORK**

# ![80%](media/symfony_black_02.png)

---

# Alejandro Hernández (@aleherse)

![inline 40%](../common/profile.jpg)

Desarrollador de aplicaciones web, consultor y formador. Trabaja actualmente en un juego web de estrategia por turnos ambientado en la antigua Grecia (+info: about.me/aleherse)

---

# ¿Qué es un framework?

>### "Un framework para aplicaciones web es un framework diseñado para **apoyar el desarrollo** de sitios web ... intenta **aliviar el exceso de carga** asociado con actividades comunes ... Por ejemplo, muchos framework **proporcionan bibliotecas** para acceder a bases de datos, estructuras para plantillas ... y con frecuencia **facilitan la reutilización de código**."
-- Wikipedia

^ framework: es una estructura conceptual y tecnológica que se usa de base para el desarrollo

---

# ¿Qué es symfony? (1/2)

Según palabras de su creador **Fabien Potencier**:

* Symfony2 is a reusable set of standalone, decoupled, and cohesive PHP components that solve common web development problems.
* Symfony2 is also a full-stack web framework.
* Symfony2 is an HTTP framework; it is a Request/Response framework.

^ Fabien Potencier fundador y líder del proyecto Symfony, cofundador de SensioLabs

^ Componentes: librerías independientes, desacopladas y cohesivas que resuelven problemas comunes del desarrollo web

^ No MVC, aunque lo es, presenta alternativas para la parte del modelo

---

# ¿Qué es symfony? (2/2)

 * Es un framework para aplicaciones web escrito en PHP.
 * Un conjunto de componentes, *más de 30*.
 * Una metodología.
 * Open Source *MIT License* y una empresa detrás *SensioLabs*.
 * Comunidad *300.000 desarrolladores en +120 paises*.
 * Usuarios *+500.000.000 de descargas*.

^ Un enfoque estructurado de desarrollo, impone una estructura, facilita muchas tareas

^ Sabes donde está la configuración, plantillas, formularios, controladores...

^ Empresa que da soporte, formación y tiene gente en plantilla dedicado al framework

^ comunidad, deSymfony conferencia reciente más de 150 asistentes

---

# Symfony Roadmap. Releases

* Una versión menor cada 6 meses (2.7, 2.8).
* Una versión mayor cada 2 años (3.0).

^ calendario claro que inspira confianza a las empresa

---

# Symfony Roadmap. Development

* 4 meses añadiendo características y mejoras.
* 2 meses de estabilización y corrección de errores.

^ dentro de los 6 meses

---

# Symfony Roadmap. Maintenance

* Versión estándar (3.1)
    * 8 meses corrigiendo errores.
    * 14 meses corrigiendo problemas de seguridad.

* Versión con soporte a largo plazo LTS (2.8)
    * 3 años corrigiendo errores.
    * 4 años corrigiendo problemas de seguridad.

---

![](https://symfony.com/doc/current/_images/release-process.jpg)

^ 2.8 ~= 3.0 mismas características

^ 3.0 elimina capa de compatibilidad con 2.x y todo aquello marcado como obsoleto

---

# Instalación

* Usando el instalador oficial

```bash
  $ sudo curl -LsS https://symfony.com /installer -o /usr/local/bin/symfony
  $ sudo chmod a+x /usr/local/bin/symfony
  $ symfony new blog 3.1
```
* Usando composer

```bash
  $ composer create-project symfony/framework-standard-edition blog "3.1"
```
---

# Configuración

Arrancar el servidor

```bash
  $ cd blog/
  $ php bin/console server:run  
```

**Configuración:** `http://localhost:8000/config.php`

**Homepage:** `http://localhost:8000/`

**Development:** `http://localhost:8000/app_dev.php`

^ configuración comprueba si nuestra máquina está preparada y qué debemos modificar

^ Accediendo al entorno dev tenemos a nuestra disposición el symfony profiler

---

### REQUEST ![inline](../common/right-arrow.png) RESPONSE

---

# Request

Objeto que encapsula las variables super globales de PHP

```php
  $request = Request::createFromGlobals();
```

```php
  $request = new Request(
      $_GET,
      $_POST,
      array(),
      $_COOKIE,
      $_FILES,
      $_SERVER
  );
```

^ no usar nunca las variables globales directamente

^ infinidad de métodos que facilitan el trabajo con esas variables

---

# Response

Objeto que encapsula toda la información que necesita el cliente

```php
  $response = new Response(
      '<html><body>Hello world!</body></html>',
      Response::HTTP_OK,
      array('content-type' => 'text/html')
  );
```

^ nunca usar echo o print

^ contiene las cabeceras: cookies, cache

^ puede usarse para redirigir al usuario, enviar ficheros, enviar json

---

### `REQUEST` ![inline](../common/right-arrow.png) ROUTER ![inline](../common/right-arrow.png) `RESPONSE`

---

# Router

Asocia una URL con una acción de un controlador

/blog/2 ![inline](../common/right-arrow.png) /blog/{page} ![inline](../common/right-arrow.png) function viewAction($page) {...}

^ ruta url => coincide con rutas existentes => función que devuelve response

^ dado una URL llama al método asociado

---

### `REQUEST` ![inline](../common/right-arrow.png) `ROUTER` ![inline](../common/right-arrow.png) CONTROLLER/ACTION ![inline](../common/right-arrow.png) `RESPONSE`

---

# Controller/Action (1/2)

Contiene funciones PHP que leen información de la Request y devuelven un objeto response

```bash
  $ php bin/console generate:controller
  > AppBundle:Blog
  > ...
```

^ AppBundle:Blog (App es el bundle por defecto)

^ Bundle: es un paquete reusable que proporciona una serie de características a las aplicaciones symfony

^ Lo recomendado es que toda tu aplicación resida en AppBundle pero dependiendo de cada caso en particular esto puede variar

---

# Controller/Action (2/2)

```php
/**
 * @Route("/", name="post_list")
 */
public function listAction()
{
    return $this->render('AppBundle:Blog:list.html.twig', array(
        // ...
    ));
}
```

^ borra el controlador por defecto y refrescar página

^ las rutas pueden tener nombre, útil para redirecciones

---

### `REQUEST` ![inline](../common/right-arrow.png) `ROUTER` ![inline](../common/right-arrow.png) `CONTROLLER/ACTION` ![inline](../common/right-arrow.png) TEMPLATE ![inline](../common/right-arrow.png)  `RESPONSE`

---

# Template

Symfony hace uso del motor de plantillas **Twig**

* **Rápido**: Las plantillas son compiladas a código PHP
* **Seguro**: El código generado es evaluado y confiable
* **Flexible**: Crea tus propias etiquetas y filtros

^ permite herencia, sintaxis sencilla

^ basado en el lenguaje de plantillas de Django

^ abrir la plantilla y hacer modificaciones

^ CSS, javascript y todo lo relacionado con frontend debe residir allí

---

### `REQUEST` ![inline](../common/right-arrow.png) `ROUTER` ![inline](../common/right-arrow.png) `CONTROLLER/ACTION` ![inline](../common/right-arrow.png) MODEL ![inline](../common/right-arrow.png) `TEMPLATE` ![inline](../common/right-arrow.png)  `RESPONSE`

---

# Model: Library (1/5)

*Doctrine 2* es una librería de mapeo objeto-relacional (ORM) para PHP 5.4+ que proporciona, de forma transparente, persistencia de objetos PHP. Utiliza el patrón **Data Mapper**.

*Propel 2* es una alternativa que utiliza el patrón **Active Record**.

`app/config/config.yml`

```yml
doctrine:
    dbal:
        driver:   pdo_mysql
        host:     "%database_host%"
```

^ Data Pattern. Intenta mantener la independencia entre el objeto y su representación en base de datos

^ Active Record. Cada objeto contiene métodos para trabajar con la base de datos

^ la configuración real de doctrine está en el fichero config.yml

---

# Model: Configuration (2/5)

Configuración de la conexión con la base de datos

`app/config/parameters.yml`

```yml
parameters:
  database_host: 127.0.0.1
  database_port: 3306
  database_name: blog
  database_user: admin
  database_password: password
```

^ parameters.yml.dist contiene la configuración general, el otro fichero es la específica a tu máquina y no debería estar en el repositorio.

---

# Model: Entity (3/5)

```bash
$ php bin/console doctrine:database:create

$ php bin/console doctrine:generate:entity
> AppBundle:Post
> ...
> Generating entity class src/AppBundle/Entity/Post.php: OK!
> Generating repository class src/AppBundle/Repository/PostRepository.php: OK!

$ php bin/console doctrine:schema:create
```

^ Entidad: objeto que tiene un ciclo de vida y será almacenado en la BD

^ añadir campos title, content y status

^ crea la clase con getters y setter y el repositorio

---

# Model: Action (4/5)

```php
$posts = $this->getDoctrine()
  ->getRepository(Post::class)
  ->findAll();

return $this->render(
  'AppBundle:Blog:list.html.twig',
  array(
    'posts' => $posts
  )
);
```

---

# Model: View (5/5)

```twig
{% extends "::base.html.twig" %}

{% block title %}Mi blog{% endblock %}

{% block body %}
    {% for post in posts %}
        <h2>{{ post.title }}</h2>
        <p>{{ post.content }}</p>
    {% else %}
        <p>No se han encontrado posts</p>
    {% endfor %}
{% endblock %}
```

---

### `REQUEST` ![inline](../common/right-arrow.png) `ROUTER` ![inline](../common/right-arrow.png) `CONTROLLER/ACTION` ![inline](../common/right-arrow.png) `MODEL` ![inline](../common/right-arrow.png) FORM ![inline](../common/right-arrow.png) `TEMPLATE` ![inline](../common/right-arrow.png)  `RESPONSE`

---

# Form: Type (1/3)

Symfony integra un componente que facilita el trabajo con formularios HTML

```bash
$ php bin/console doctrine:generate:form AppBundle:Post
```

```php
$builder
    ->add('title')
    ->add('content')
    ->add('status', ChoiceType::class, array(
        'choices'  => array('borrador' => 0, 'Publicado' => 1),))
    ->add('guardar', SubmitType::class);
```

^ genera el formulario asociado con una entidad

^ se pueden añadir gran cantidad de validaciones al formulario

---

# Form: Action (2/3)

```PHP
  $form = $this->createForm(PostType::class);
  $form->handleRequest($request);

  if ($form->isSubmitted() && $form->isValid()) {
      $manager = $this->getDoctrine()->getManager();

      $manager->persist($form->getData());
      $manager->flush();

      return $this->redirectToRoute("add_post");
  }

  return $this->render('AppBundle:Blog:add.html.twig', array(
      'form' => $form->createView()
  ));
```

^ las acciones de administración deberían estar en otro controlador

---

# Form: View (3/3)

```Twig
{% extends "::base.html.twig" %}

{% block title %}Añadir post{% endblock %}

{% block body %}
    {{ form_start(form) }}
    {{ form_widget(form) }}
    {{ form_end(form) }}
{% endblock %}
```

^ Twig nos permite personalizar el html de todos los elementos del formulario

---

### `REQUEST` ![inline](../common/right-arrow.png) `ROUTER` ![inline](../common/right-arrow.png) SECURITY ![inline](../common/right-arrow.png) `CONTROLLER/ACTION` ![inline](../common/right-arrow.png) `MODEL` ![inline](../common/right-arrow.png) `FORM` ![inline](../common/right-arrow.png) `TEMPLATE` ![inline](../common/right-arrow.png)  `RESPONSE`

---

# Security (1/2)

Si una request referencia a un área protegida:

* Extraer token con credenciales de la request
* Validar token y devolver un token autenticado
* Guardar el token autenticado
* Permitir el acceso

---

# Security (2/2)

```yml
  providers:
      in_memory:
          memory:
              users:
                  admin:
                      password: password
                      roles: 'ROLE_ADMIN'
  encoders:
      Symfony\Component\Security\Core\User\User: plaintext

  firewalls:
      main:
          anonymous: ~
          http_basic: ~

  access_control:
      - { path: ^/admin, roles: ROLE_ADMIN }
```

^ in memory es el un authentication manager

^ se puede usar un formulario, LDAP, algo específico a tu negocio...

^ no usar NUNCA plaintext como encoder

^ cambiar ruta y demo usando el profiler

---

# A tener en cuenta

* Symfony es muy potente y este ejemplo muy simple
* El controlador por defecto contiene mucha magia
* Crea servicios y no pongas negocio en el controlador
* Seguro que hay un bundle para eso (packagist.org)
* No tengas miedo en navegar por el código de Symfony

^ se pueden crear controladores como servicios para controlar las dependencias

^ ver cómo funciona internamente te permite entender y descubrir cómo hacer cosas

^ ejemplo de bundle con https://github.com/javiereguiluz/EasyAdminBundle/blob/master/Resources/doc/getting-started.md

---

# ¿Por dónde continuar?

* **Documentación** (symfony.com/doc/current)
* **Best Practices** (symfony.com/doc/current/best_practices)
* **Bundles** (symfony.com/doc/bundles)
* **Desarrollo web ágil con Symfony2** (symfony.es/libro)
* **Charlas SensioLabs** (youtube.com/user/SensioLabs)
* **Charlas deSymfony** (youtube.com/user/desymfony)

---

# Enlaces de interés

* http://symfony.com
* http://silex.sensiolabs.org
* http://fabien.potencier.org/what-is-symfony2.html
* https://github.com/symfony/symfony-demo
* http://twig.sensiolabs.org
* http://symfony.es
* http://sylius.org

---

# ¿Preguntas?
