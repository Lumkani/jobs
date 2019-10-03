## Component Model

A component model is a model that describes the way we should build our components in our Vue-based applications. It is heavily based off the [Atomic Design](http://bradfrost.com/blog/post/atomic-web-design/) methodology

<small><i>Component Model Diagram</i></small>

![Component Model Diagram](/component-model.png)

* **Atom**: The smallest component possible, these are usually buttons, labels, icons and so fourth.
* **Molecule**: These are contextual components that are made of one, two, or three atoms. They usually form basic inputs, media cards and so fourth.
* **Organism**: These form a distinct section of your page, such as headers, complex forms, data tables and etc...
* **Page**: Used as an entry point for all organisms. Your page component should only be composed of organisms.
* **Web**: Responsible for providing context to the page, such as routing.

:::tip
**Atoms** and **Molecules** are the most reusable components out of the **Component Model** as both of them have the least amount of context. They tend to have very little props.
:::

Here at Lumkani we use Quasar as a UI Component library, so that means that it already provides us with *Atoms*, *Molecules* and possibly *Organisms*.

In most cases we would probably want to customize/extend some of the Quasar components to suite our needs. So if we want to extend a Molecule, we would just need to create a new Molecule with our necessary features.

For example, lets say we have a Quasar component called `QInput` (which is a molecule) and I want to add some custom features to it and we called it `LumkaniInput`, it should still be a molecule. So it's very important to keep in mind in what way you are extending components as the Component Model tries to create a framework for reasoning about when to create components