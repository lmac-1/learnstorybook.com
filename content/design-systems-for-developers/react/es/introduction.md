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

## What’s all the fuss about design systems anyways?

Let’s get something out of the way: the concept of a reusable user interface isn’t new. Styleguides, UI kits, and shareable widgets have existed for decades. Today, designers and developers are aligning towards the UI component construct. A UI component encapsulates the visual and functional properties of discrete user interface pieces. Think LEGO bricks.

Modern user interfaces are assembled from hundreds of modular UI components that are rearranged to deliver different user experiences.

Design systems contain reusable UI components that help teams build complex, durable, and accessible user interfaces across projects. Since both designers and developers contribute to the UI components, the design system serves as a bridge between disciplines. It is also the “source of truth” for an organization’s common components.

![Design systems bridge design and development](/design-systems-for-developers/design-system-context.jpg)

Designers often talk about building design systems inside their tools. The holistic scope of a design system encompasses assets (Sketch, Figma, etc.), overarching design principles, contribution structure, governance, and more. There’s an abundance of designer-oriented guides that dive deep into these topics so we won’t rehash that here.

For developers, a few things are certain. Production design systems must include the UI components and the frontend infrastructure behind it all. There are three technical parts to a design system that we’ll talk about in this guide:

- 🏗 Common reusable UI components
- 🎨 Design tokens: Styling-specific variables such as brand colors and spacing
- 📕 Documentation site: Usage instructions, narrative, do’s and don'ts

The parts are packaged up, versioned, and distributed to consumer apps via a package manager.

## Do you need a design system?

Despite the hype, a design system isn’t a silver bullet. If you work with a modest team on a single app, you’re better off with a directory of UI components instead of setting up the infrastructure to enable a design system. For small projects, the cost of maintenance, integration, and tooling far outweighs any productivity benefits you might see.

The economy of scale in a design system works in your favor when sharing UI components across many projects. If you find yourself pasting the same UI components in different apps or across teams, this guide is for you.

## What we’re building

Storybook powers the design systems for [BBC](https://www.bbc.co.uk/iplayer/storybook/index.html?path=/story/style-guide--colours), [Airbnb](https://github.com/airbnb/lunar), [IBM](https://www.carbondesignsystem.com/), [GitHub](https://primer.style/css/), and hundreds more companies. The recommendations here are inspired by best practices and tools from the smartest teams. We’ll be building the following frontend stack:

#### Build components

- 📚 [Storybook](http://storybook.js.org) for UI component development and auto-generated docs
- ⚛️ [React](https://reactjs.org/) for declarative component-centric UI (via create-react-app)
- 💅 [Emotion](https://emotion.sh/docs/introduction) for component-scoped styling
- ✨ [Prettier](https://prettier.io/) for automatic code formatting

#### Maintain the system

- 🚥 [GitHub Actions](https://github.com/features/actions) for continuous integration
- 📐 [ESLint](https://eslint.org/) for JavaScript linting
- ✅ [Chromatic](https://www.chromatic.com/?utm_source=storybook_website&utm_medium=link&utm_campaign=storybook) to catch visual bugs in components (by Storybook maintainers)
- 📦 [npm](https://npmjs.com) for distributing the library
- 🛠 [Auto](https://github.com/intuit/auto) for release management workflow

#### Storybook addons

- ♿ [Accessibility](https://github.com/storybookjs/storybook/tree/master/addons/a11y) to check for accessibility issues during development
- 💥 [Actions](https://storybook.js.org/docs/react/essentials/actions) to QA click and tap interactions
- 🎛 [Controls](https://storybook.js.org/docs/react/essentials/controls) to interactively adjust props to experiment with components
- 📕 [Docs](https://storybook.js.org/docs/react/writing-docs/introduction) for automatic documentation generation from stories
- 🔍 [Interactions](https://storybook.js.org/addons/@storybook/addon-interactions/) for debugging component interactions
- 🏎 [Test-runner](https://storybook.js.org/docs/react/writing-tests/test-runner) for automated component testing

![Design system workflow](/design-systems-for-developers/design-system-workflow.jpg)

## Understand the workflow

Design systems are an investment in frontend infrastructure. In addition to showcasing how to use the technology above, this guide also focuses on core workflows that promote adoption and simplify maintenance. Wherever possible, manual tasks will be automated. Below are the activities we’ll encounter.

#### Build UI components in isolation

Every design system is composed of UI components. We’ll use Storybook as a “workbench” to build UI components in isolation outside of our consumer apps. Then we’ll integrate timesaving addons that help you increase component durability (Actions, A11y, Controls, Interactions).

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
