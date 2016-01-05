---
layout: post
title: La piratería de la que no se habla
date: '2011-02-03'
---

La palabra piratería se utiliza sin mucho contexto para hablar de todo tipo de violaciones a las leyes de [derechos de autor](http://es.wikipedia.org/wiki/Derecho_de_autor), de la misma manera que la palabra “propiedad intelectual” se utiliza para confundir a la gente discutiendo juntos temas tan distintos como patentes y derechos de autor.

En el mundo del software, se crearon grupos como la [BSA](http://www.bsa.org/) (Business Software Alliance) o la local [ADS](http://www.ads.cl/), las cuales se publicitan como grupos en defensa de la causa misma, pero en realidad sólo trabajan para las empresas que las integran. Es por eso que no verás a estas agrupaciones condenar violaciones de copyright al software libre.

## ¿Piratear software libre? ¿No que es gratis?

El software libre es una obra como cualquiera otra. Está protegida por el derecho de autor, lo que significa que el autor es dueño y se reserva todos los derechos sobre la obra. Sin embargo, en el caso del software libre, este añade una “licencia” que permite ciertos usos. Hay [varias licencias populares](http://es.wikipedia.org/wiki/Licencia_de_software_libre). En el caso del software distribuido bajo [la licencia GPL](http://www.viti.es/gnu/licenses/gpl.html), se permite la copia, venta, modificación y redistribución bajo la condición de que el que distribuye mantenga todas estas libertades. Esto incluye por ejemplo, acceso al código fuente modificado.

Si se incumple la licencia, entonces se pierde toda autorización sobre el software (entra el juego el “Todos los derechos reservados” del que disfruta el autor).

Dichos incumplimientos son más comunes de lo que se piensa. Hoy en día una gran cantidad de aparatos electrónicos de consumo llevan Linux dentro: celulares, reproductores de DVD, televisores LCD, Tablets etc. Algunos ejemplos:

- De los tablets con Android, se mostró como [casi ninguno cumplía](http://projectgus.com/2010/07/open-source-in-android-tablets/) con las licencias libres.
- Skype estuvo involucrado [en un juicio](http://www.h-online.com/newsticker/news/item/GPL-violation-by-Skype-re-affirmed-by-court-735143.html) por similar infracción y fue declarado culpable.

No sólo a empresas les ha pasado. Tanto [España como Portugal](http://www.osor.eu/news/administrations-in-spain-and-portugal-heckled-over-licence-violations) violaron las licencias un software utilizado en la implementación de sus documentos de identidad electrónicos.

Las infracciónes siguen un patrón. Una empresa u organismo desarrolla un nuevo producto. Utilizan software libre como base para ahorrarse costes, ignorando sus consecuencias. A veces sin saber, por ejemplo cuando subcontratan el desarrollo a un tercero. Es lo [que le sucedió a Atari](http://www.h-online.com/open/news/item/Atari-settle-over-ScummVM-based-Wii-game-GPL-violations-742191.html), que terminó llegando a un acuerdo con los autores del motor de videojuegos [ScummVM](http://es.wikipedia.org/wiki/ScummVM) , luego de que un juego que Atari subcontrató había sido desarrollado sobre este motor sin cumplir su licencia. Para peor, Nintendo prohibía explícitamente los juegos Wii con componentes de código abierto, por lo que Atari en un principio negó el problema, e incluso amenazó legalmente a los autores de ScummVM.

Muchas de estas condiciones son especificas a la licencia GPL. Otras licencias son más permisivas. Por ejemplo, el sistema [OSX de Apple está ba](http://es.wikipedia.org/wiki/Darwin_BSD) [sado](http://es.wikipedia.org/wiki/Darwin_BSD) en software abierto el cual no requiere que Apple publique sus modificaciones. Sin embargo, el motor que hacer funcionar su navegador Safari, sí [está basado en códigos](http://es.wikipedia.org/wiki/WebKit) que obligan a Apple a publicar sus modificaciones.

Cabe destacar que la mayoría de las veces esto sucede por simple ignorancia. Es por esto que la estrategia de las organizaciones que representan a los autores en los litigios contra grandes empresas en general buscan solucionar el problema: que se respete la licencia (por ejemplo: que se publiquen los códigos modificados). Sin embargo, dado el efecto “David contra Goliat”, muchas empresas se dan el lujo de ignorar cualquier llamada de atención. Esto empieza a cambiar, dado que hoy en día los dueños de muchos derechos de autor en el mundo del software libre son también grandes empresas (IBM, Google, etc).

## “Pato Cable”…. “Pato Programa” u “Hola Pato”

En Chile también hay casos. Hace varias semanas [saltó a la luz](http://blog.cristianrodriguez.net/2011/01/vtr-hola-y-la-licencia-gpl.html) que el programa [Hola de VTR](http://www.holavtr.cl/descarga-hola.php) era en realidad una versión modificada de [Qutecom](http://www.qutecom.org/), el cual se distribuye bajo la licencia GPL. El problema no es sólo la falta del código modificado, sino que VTR [cambió el archivo donde estaba escrita la licencia](http://jci.codemonkey.cl/index.php/2011/01/13/are-you-fucking-kidding-me) y lo reemplazó con su propio texto legal, efectivamente cambiando la licencia original y borrando de paso todas las libertades, y por ende, perdiendo ellos todo permiso sobre el software.

Personalmente envié un correo a los contactos de prensa de VTR (Mireya Leyton y Ana Olate) y no recibí respuesta alguna. Un empleado de VTR comentó que había informado del tema a la gerencia correspondiente, pero el “Hola” pirata sigue allí.

Los autores de Qutecom [están buscando ayuda legal](http://www.groklaw.net/comment.php?mode=display&sid=20110114185542531&title=GPL%20Violation%20help%20needed&type=article&order=&hideanonymous=0&pid=896068#c896247), lo cual será difícil debido a que es una empresa muy pequeña, por lo que seguramente tendrán que recurrir a organizaciones internacionales que defienden la propiedad intelectual que no le interesa a la BSA o a la ADS. ¿Caso perdido?.

Al parecer no. Qutecom utiliza las bibiliotecas Qt, ahora propiedad de Nokia. La versión incluida en “Hola” es la 4.3 la cual tiene dos versiones: proprietaria (la cual hay que pagar) o GPL. VTR utilizó esta última, y al distribuir “Hola” bajo una licencia distinta a la GPL (al cambiar los textos) han pasado también a violar los derechos de autor de Nokia, lo cual lo hace una travesura bastante más peligrosa.

## Copiar un sitio web … Yes we can!

El colmo de estos casos es cuando los infractores son los mismos que están a cargo de legislar.

Un caso que salió a la luz fue el del [sitio web del diputado Nicolás Monckeberg](http://www.nicolasmonckeberg.cl/):

 ![]({{ site.baseurl }}/assets/images/posts/nicolas_website-scaled500.png)

el cual tenía cierto parecido con el sitio del presidente Obama!:

**(update: lamentablemente mi blog no sobrevivió y perdí la foto. Obama entre tanto ya tiene un nuevo diseño)**

 ![]({{ site.baseurl }}/assets/obama-site-106x300.png)

Cabe destacar que aquí no estamos hablando de parecidos ni inspiraciones. Un análisis del código del sitio web del diputado revela que el sitio es una copia. Identificadores del sitio de Obama como “getinvolved\_calltovolunteer” aparecen tal cual en el sitio de Monckeberg.

Contacté al diputado vía email y facebook, sin recibir respuesta alguna.

## Conclusiones

Hace unas semanas, un jóven murió en [el incendio de San Miguel](http://es.wikipedia.org/wiki/Incendio_en_la_c%C3%A1rcel_de_San_Miguel) (Cárcel en Santiago de Chile). El estaba allí por vender películas piratas. Cuando se trata de defender los intereses de ciertos grupos, la ley funciona muy rápido.

El software libre tiene derechos de autor, y el autor es dueño de la obra, la cual distribuye libremente con la condición de que sus condiciones sean respetadas.

Espero que en el futuro, cuando hablemos de piratería, no estemos sólo discutiendo acerca de alguien vendiendo DVDs en la calle, o a un jóven descargando música desde una red P2P, sino que también te imagines a los ejecutivos de grandes compañías, fabricantes de electrónica, e incluso parlamentarios. La diferencia esta en que ningun juez enviaría a estos últimos a la Cárcel de San Miguel.

