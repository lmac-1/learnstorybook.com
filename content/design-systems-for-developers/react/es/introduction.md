---
title: 'Introducción a los sistemas de diseño'
tocTitle: 'Introducción'
description: 'Una guía de las últimas herramientas listas para producción para sistemas de diseño'
---

<div class="aside">Esta guía está dirigida a <b>desarrolladores profesionales</b> que quieren aprender a construir sistemas de diseño. Se recomienda experiencia intermedia en JavaScript, Git, e integración continua. También deberías conocer los conceptos básicos de Storybook, como escribir una historia y editar archivos de configuración (<a href="/es/intro-to-storybook">la Introducción a Storybook</a> enseña los fundamentos).
</div>
<br/>

Los sistemas de diseño están ganando popularidad rápidamente. Desde gigantes tecnológicos como Airbnb hasta startups ágiles, organizaciones de todo tipo están reutilizando patrones de interfaz de usuario (UI) para ahorrar tiempo y dinero. Pero hay una gran distancia entre los sistemas de diseño creados por BBC, Airbnb, IBM o Microsoft y los sistemas de diseño creados por la mayoría de los desarrolladores.

¿Por qué los equipos de sistemas de diseño líderes utilizan las herramientas y técnicas que utilizan? Mi coautor Tom y yo investigamos las características de los sistemas de diseño exitosos de la comunidad de Storybook para identificar las mejores prácticas.

Esta guía paso a paso revela las herramientas automatizadas y los flujos de trabajo cuidadosos utilizados en sistemas de diseño de producción a escala. Recorreremos el proceso de ensamblar un sistema de diseño a partir de bibliotecas de componentes existentes, y luego configuraremos servicios principales, bibliotecas y flujos de trabajo.

![Design system overview](/design-systems-for-developers/design-system-overview.jpg)

## ¿Por qué tanto interés en los sistemas de diseño?

Vamos a aclarar algo: el concepto de una interfaz de usuario reutilizable no es nuevo. Las guías de estilo, kits de UI y widgets compartibles han existido durante décadas. Hoy en día, diseñadores y desarrolladores están alineándose hacia la construcción de componentes de interfaz de usuario (UI). Un componente de UI encapsula las propiedades visuales y funcionales de las piezas discretas de la interfaz de usuario. Piensa en bloques de LEGO.

Las interfaces de usuario modernas se ensamblan a partir de cientos de componentes modulares de UI que se reorganizan para ofrecer diferentes experiencias de usuario.

Los sistemas de diseño contienen componentes de UI reutilizables que ayudan a los equipos a construir interfaces de usuario complejas, duraderas y accesibles en diferentes proyectos. Ya que tanto diseñadores como desarrolladores contribuyen a los componentes de UI, el sistema de diseño sirve como un puente entre disciplinas. También es la “fuente de verdad” para los componentes comunes de una organización.

Design systems contain reusable UI components that help teams build complex, durable, and accessible user interfaces across projects. Since both designers and developers contribute to the UI components, the design system serves as a bridge between disciplines. It is also the “source of truth” for an organization’s common components.

![Design systems bridge design and development](/design-systems-for-developers/design-system-context.jpg)

Los diseñadores a menudo hablan de construir sistemas de diseño dentro de sus herramientas. El alcance holístico de un sistema de diseño abarca activos (Sketch, Figma, etc.), principios de diseño generales, estructura de contribución, gobernanza y más. Existen abundantes guías orientadas a diseñadores que profundizan en estos temas, por lo que no repetiremos eso aquí.

Para los desarrolladores, hay algunas cosas seguras. Los sistemas de diseño en producción deben incluir los componentes de UI y la infraestructura frontend que los respalda. Hay tres partes técnicas de un sistema de diseño que hablaremos en esta guía:

- 🏗 Componentes de UI comunes y reutilizables
- 🎨 Tokens de diseño: Variables específicas de estilo como colores de marca y espaciado
- 📕 Sitio de documentación: Instrucciones de uso, narrativa, lo que se debe y no se debe hacer

Las partes se empaquetan, se versionan y se distribuyen a las aplicaciones consumidoras a través de un gestor de paquetes.

## ¿Necesitas un sistema de diseño?

A pesar del boom, un sistema de diseño no es una solución mágica. Si trabajas con un equipo modesto en una sola aplicación, es mejor contar con un directorio de componentes de UI en lugar de configurar toda la infraestructura para habilitar un sistema de diseño. En proyectos pequeños, el costo de mantenimiento, integración y herramientas supera con creces cualquier beneficio de productividad que puedas obtener.

La economía de escala de un sistema de diseño juega a tu favor cuando compartes componentes de UI en varios proyectos. Si te encuentras copiando y pegando los mismos componentes de UI en diferentes aplicaciones o entre equipos, esta guía es para ti.

## Lo que estamos construyendo

Storybook potencia los sistemas de diseño de [BBC](https://www.bbc.co.uk/iplayer/storybook/index.html?path=/story/style-guide--colours), [Airbnb](https://github.com/airbnb/lunar), [IBM](https://www.carbondesignsystem.com/), [GitHub](https://primer.style/css/), y cientos de otras empresas. Las recomendaciones aquí están inspiradas en las mejores prácticas y herramientas de los equipos más inteligentes. Vamos a construir la siguiente pila frontend:

#### Componentes de construcción

- 📚 [Storybook](http://storybook.js.org) para el desarollo de componentes UI y documentación generada automáticamente
- ⚛️ [React](https://reactjs.org/) para una UI centrada en componentes declarativos (a través de create-react-app)
- 💅 [Emotion](https://emotion.sh/docs/introduction) para estilos específicos de componentes
- ✨ [Prettier](https://prettier.io/) para el formateo automático del código

#### Mantener el sistema

- 🚥 [GitHub Actions](https://github.com/features/actions) para integración continua
- 📐 [ESLint](https://eslint.org/) para linting de JavaScript
- ✅ [Chromatic](https://www.chromatic.com/?utm_source=storybook_website&utm_medium=link&utm_campaign=storybook) para detectar errores visuales en los componentes (por los mantenedores de Storybook)
- 📦 [npm](https://npmjs.com) para distribuir la librería
- 🛠 [Auto](https://github.com/intuit/auto) para el flujo de trabajo de gestión de lanzamientos

#### Complementos (addons) de Storybook

- ♿ [Accesibilidad](https://github.com/storybookjs/storybook/tree/master/addons/a11y) para verificar problemas de accesibilidad durante el desarrollo
- 💥 [Acciones](https://storybook.js.org/docs/react/essentials/actions) para el aseguramiento de calidad (QA) de interacciones de clic y toque
- 🎛 [Controles](https://storybook.js.org/docs/react/essentials/controls) para ajustar interactivamente las props y experimentar con los componentes
- 📕 [Docs](https://storybook.js.org/docs/react/writing-docs/introduction)  para la generación automática de documentación a partir de historias
- 🔍 [Interacciones](https://storybook.js.org/addons/@storybook/addon-interactions/) para depurar interacciones de componentes
- 🏎 [Test-runner](https://storybook.js.org/docs/react/writing-tests/test-runner) para pruebas automatizadas de componentes

![Design system workflow](/design-systems-for-developers/design-system-workflow.jpg)

## Comprender el flujo de trabajo

Los sistemas de diseño son una inversión en infraestructura frontend. Además de mostrar cómo utilizar la tecnología mencionada, esta guía también se centra en los flujos de trabajo esenciales que promueven la adopción y simplifican el mantenimiento. Siempre que sea posible, se automatizarán las tareas manuales. A continuación se presentan las actividades que encontraremos.

#### Construir componentes de UI de forma aislada

Cada sistema de diseño se compone de componentes de UI. Usaremos Storybook como un 'taller' para construir componentes de UI de manera aislada, fuera de nuestras aplicaciones consumidoras. Luego, integraremos complementos que ahorran tiempo y te ayudarán a aumentar la durabilidad de los componentes (Acciones, A11y, Controles, Interacciones).

#### Review to reach consensus and gather feedback

UI development is a team sport that requires alignment between developers, designers, and other disciplines. We’ll publish work-in-progress UI components to loop stakeholders into the development process so we can ship faster.

#### Test to prevent UI bugs

Design systems are a single source of truth and a single point of failure. Minor UI bugs in basic components can snowball into company-wide incidents. We’ll automate tests to help you mitigate the inevitable bugs to ship durable, accessible UI components with confidence.

#### Document to accelerate adoption

Documentation is essential, but creating it is often a developer’s last priority. We’ll make it much easier for you to document UI components by auto-generating minimum viable docs which can be further customized.

#### Distribute the design system to consumer projects

Once you have well-documented UI components, you need to distribute them to other teams. We’ll cover packaging, publishing, and how to surface the design system in other Storybooks.

## Storybook Design System

This guide’s example design system was inspired by Storybook’s own [production design system](https://github.com/storybookjs/design-system). It is consumed by three sites and touched by tens of thousands of developers in the Storybook ecosystem.

In the next chapter, we’ll show you how to extract a design system from disparate component libraries.
