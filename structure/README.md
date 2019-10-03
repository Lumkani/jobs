## Component Model

A component model is model that describes the way we should build our components in our Vue-based applications.

<small><i>Component Model Diagram</i></small>

![Component Model Diagram](/component-model.png)

* **Atom**: The smallest component possible, these are usually buttons, labels, icons and so fourth.
* **Molecule**: These are contextual components that are made of one, two, or three atoms. They usually form basic inputs, media cards and so fourth.
* **Organism**: These form a distinct section of your page, such as headers, complex forms, data tables and etc...
* **Page**: Used as an entry point for all organisms.
* **Web**: Responsible for providing context to the page, such as routing.
