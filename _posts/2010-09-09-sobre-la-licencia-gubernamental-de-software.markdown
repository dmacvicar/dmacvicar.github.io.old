---
layout: post
title: Sobre la Licencia Gubernamental de Software
date: '2010-09-09'
tags:
- chile
- government
- software
---

Pedro Huichalaf Roa publicó [una columna](http://www.culturadigital.cl/wp/?p=1464) donde explica más detalladamente la [“Licencia Gubernamental de Software”](http://www.estrategiadigital.gob.cl/files/LicenciamientoGubernamentaldeSoftwarev1.pdf), antes conocida con el (muy mal escogido) nombre de GPL-CL.

La columna nos dá el contexto que faltó cuando la información se comenzó a propagar por las redes sociales. Con su super-nombre (GPL-CL) estó sucedió más rápido de lo necesario, generando una polémica innecesaria.

Sin embargo, ahora que se ve que no era necesario polemizar, creo más que nunca que es un tema importante y que ojalá no se lleve a cabo como está.

Muchos de los puntos críticos de la licencia fueron muy bien explicados en [la carta abierta](http://www.cdsl.cl/portal/content/carta-abierta-al-gobierno-de-chile-licenciamiento-gubernamental-de-software) que envió [El Centro de Difusión de Software Libre (CDSL)](http://www.cdsl.cl/), el [Portal Software Libre Chile](http://www.softwarelibre.cl/), el [Partido Pirata de Chile](http://www.partidopirata.cl/) y [Fundación GNUCHILE](http://www.fundaciongnuchile.cl/) y en el [análisis técnico](http://eldiabloenlosdetalles.net/2010/09/04/anatomia-de-la-licencia-gubernamental-de-software) publicado por eldiabloenlosdetalles.net, por lo que sólo voy a tocar los puntos que me tienen confundido.

También quisiera aclarar que no soy abogado. Sin embargo he publicado bastante código fuente bajo varias licencias (GPL, BSD, etc) siendo en algunos casos yo el dueño del copyright como la empresa donde trabajo en otros.

Citando el texto de Huichalaf:

> Para entender este nuevo ecosistema, tenemos que partir de la base que la transferencia de tecnología se basa en aquella tecnología cuya propiedad le corresponde al Estado. Es decir, estamos hablando de aquellos software y desarrollos informáticos realizados “a medida” para organizaciones de la Administración Pública y cuya propiedad intelectual posea la entidad. Así, un ejemplo es un software desarrollado para gestión documental o calificación de personal.

En este parrafo, se deja claro que estamos hablando de desarrollos donde el Estado es el dueño del copyright.

> Lo que ocurre actualmente en relación a este tipo de software a la medida, es que las entidades públicas para efectos de adquisición y desarrollo se comportan en forma autónoma sin que exista comunicación centralizada respecto a soluciones adquiridas.

¿En qué quedamos? ¿Son las agencias gubernamentales la misma organización: el Estado, o no?. El raciocinio se debe hacer sólo con una de ambas hipótesis.

> Justamente en este sentido es que nace la idea de generar un repositorio centralizado de software desarrollado para la Administración Pública y cuyos derechos de autor le corresponden al Estado , lo que se concreta en softwarepublico.cl y al mismo tiempo, surge la Licencia Gubernamental de Software, que en términos simples, establece las condiciones de uso del software del Estado.

Aquí empieza la confusión. Si esta licencia es para uso interno del gobierno entre sus entidades, cada entidad puede hacer uso de los softwares sin mayor problema, a menos que el contrato por parte del desarrollador externo no deje al Estado como dueño del copyright, sino que como un mero usuario de una licencia, lo cual contradice el parrafo inicial de Huichalaf (Estado como dueño del copyright). El Estado no necesita entrar en un sistema de distribución de software. El problema de la distribución interna es algo organizacional y político, pero (repito no soy abogado) no algo legal.

Si el fin es compartir entre entidades, ¿Por qué la licencia es tan permisiva?, básicamente permitiendo la distribución de un modo muy similar a la GPL (sin restricciones más que conservar las condiciones, y una restricción agregada de registrarse en softwarepúblico.cl). Aquí surge la confusión, y pareciera que la licencia tiene un objetivo más abierto que el compartir entre entidades de una misma organización. Si esto no fuese así, y la idea es que sea permisiva, volvemos al ¿Por qué no usar la GPL?.

Cuando se trata de ser dueño del copyright, el Estado es uno. Cuando se trata de distribuirlo internamente, ya no es una sola entidad. ¿Cómo es la cosa?

Luego la segunda parte que no calza en el puzzle. ¿Para qué se publica en softwarepublico.cl y no se mantiene en una intranet del gobierno?. Si el objetivo es solo compartirlo entre entidades, esto es ilógico. Si el objetivo es compartirlo con la ciudadanía (lo cual no estaría mal, siendo que ella paga por él) esto tendría sentido.

Si este repositorio es accedido solo por entidades del estado, entonces no habría problema. El problema es cuando se comparte entre países, lo cual es una gran oportunidad, o cuando contratistas del Gobierno reciben este software como modo de externalizar un servicio. Ahí pasamos a la distribución, y por ende se activan las incompatibilidades con la GPL, y por ende no se podrá (por citar un ejemplo) usar la librería del lado “cliente” de MySQL.

Esto contradice ahora la estrategia de Alfredo Barriga de querer externalizar algunos servicios en la “nube”.

Por ejemplo, si un sistema de soporte IT desarrollado a medida bajo esta licencia, se externalizara a un contratista, se estaría incurriendo en distribución.

Hasta aquí tampoco entiendo por qué no utilizar una licencia absolutamente libre, ya que en efectos prácticos esta licencia permitiría a cualquiera que tenga acceso al software a seguir distribuyendolo como fuentes.

Resumiendo:

- Una licencia nueva es una pésima idea que trae consigo más problemas que soluciones, y estos problemas permanecerán ocultos hasta que una gran oportunidad se vea bloqueada por ellos.
- No se entiende por qué una licencia tan permisiva siendo el objetivo una distribución en un entorno cerrado, y por ende por qué no utilizar la GPL de una vez.
- La idea confunde licencia y distribución. Se podría utilizar la GPLv3 y simplemente no distribuir fuera del gobierno lo que no sea para distribuir, o dejarlo como propietario para uso exclusivo del gobierno (y [controlar a los contratistas con el contrato, no con la licencia](http://www.gnu.org/licenses/gpl-faq.html#DoesTheGPLAllowNDA)). Lo que se va a distribuir, se puede poner en [http://www.gobforge.gob.cl/](http://www.gobforge.gob.cl/), el cual no se que rol tiene en todo este tema de la nueva licencia o que criterio se utiliza para poner software en un lado o en el otro (más confusión aún).
- Creo que el plan confunde el ordenar la casa, los contratos, las condiciones, etc., con una licencia nueva: Persiguiendo un objetivo que con sus mismos defectos destruye.

Me gustaría saber ¿Por qué no se podría usar la GPLv3 más un proceso y política clara y explícita respecto a distribución, contratos para contratistas, desarrollo externo, licencias a utilizar en cada caso, etc.

Yendo más allá, creo que la licencia no es el paso más importante. Siento más urgente y me hubiese gustado ver directivas. En especial cuando hablamos de desarrollo a medida por terceros:

- Que obliguen el uso de software libre en los softwares hechos a medida excepto en casos estrictamente justificados (costo, razones técnicas).
- Que regulen y expliquen el como los datos de los ciudadanos se van a almacenar en servicios externos. Aquí en Alemania está prohibido sacarlos del país por ejemplo.
- El uso de estándares: acceder a la información del gobierno, datos, etc en forma transparente: [Las APIs de Obama](http://www.data.gov/), o la discriminación que uno sufre cuando te obligan a enviar datos en formato .doc.
