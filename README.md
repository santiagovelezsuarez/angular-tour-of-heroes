# AngularTourOfHeroes

This project was generated with [Angular CLI](https://github.com/angular/angular-cli) version 12.2.7.

## Development server

Run `ng serve` for a dev server. Navigate to `http://localhost:4200/`. The app will automatically reload if you change any of the source files.

## Code scaffolding

Run `ng generate component component-name` to generate a new component. You can also use `ng generate directive|pipe|service|class|guard|interface|enum|module`.

## Build

Run `ng build` to build the project. The build artifacts will be stored in the `dist/` directory.

## Running unit tests

Run `ng test` to execute the unit tests via [Karma](https://karma-runner.github.io).

## Running end-to-end tests

Run `ng e2e` to execute the end-to-end tests via a platform of your choice. To use this command, you need to first add a package that implements end-to-end testing capabilities.

## Further help

To get more help on the Angular CLI use `ng help` or go check out the [Angular CLI Overview and Command Reference](https://angular.io/cli) page.

#Tutorial Steps
###Change the application title
app.component.ts (class title property) |
--- |
`title = 'Tour of Heroes';` |

app.component.html (template) |
--- |
`<h1>{{title}}</h1>` |

###Add application styles
src/styles.css (excerpt) |
--- |
```
/* Application-wide Styles */ |
h1 {
  color: #369;
  font-family: Arial, Helvetica, sans-serif;
  font-size: 250%;
}
h2, h3 {
  color: #444;
  font-family: Arial, Helvetica, sans-serif;
  font-weight: lighter;
}
body {
  margin: 2em;
}
body, input[type="text"], button {
  color: #333;
  font-family: Cambria, Georgia, serif;
}
/* everywhere else */
* {
  font-family: Arial, Helvetica, sans-serif;
}
```


###Create the heroes component
`$ ng generate component heroes`

- Add a hero property to the HeroesComponent for a hero named "Windstorm."

heroes.component.ts (hero property) |
--- |
`hero = 'Windstorm';` |

heroes.component.html |
--- |
`<h2>{{hero}}</h2>` |

src/app/app.component.html |
--- |
```
<h1>{{title}}</h1>
<app-heroes></app-heroes>
```

###Create a Hero interface
src/app/hero.ts |
--- |
```
export interface Hero 
{
  id: number;
  name: string;
}
```
Return to the HeroesComponent class and import the Hero interface.
src/app/heroes/heroes.component.ts |
--- |
```
import { Component, OnInit } from '@angular/core';
import { Hero } from '../hero';

@Component({
  selector: 'app-heroes',
  templateUrl: './heroes.component.html',
  styleUrls: ['./heroes.component.css']
})
export class HeroesComponent implements OnInit {
  hero: Hero = {
    id: 1,
    name: 'Windstorm'
  };

  constructor() { }

  ngOnInit() {
  }

}
```
###Show the hero object
heroes.component.html (HeroesComponent's template) |
--- |
```
<h2>{{hero.name}} Details</h2>
<div><span>id: </span>{{hero.id}}</div>
<div><span>name: </span>{{hero.name}}</div>
```
###Format with the UppercasePipe
Modify the `hero.name` binding like this.
src/app/heroes/heroes.component.html |
--- |
```
<h2>{{hero.name | uppercase}} Details</h2>
```
**Pipes** are a good way to format strings, currency amounts, dates and other display data. Angular ships with several built-in pipes and you can create your own.

###Edit the hero
Users should be able to edit the hero name in an `<input>` textbox.

setup a **two-way data binding** between the `<input>` form element and the `hero.name` property.
###Two-way binding
src/app/heroes/heroes.component.html (HeroesComponent's template) |
--- |
```
<div>
  <label for="name">Hero name: </label>
  <input id="name" [(ngModel)]="hero.name" placeholder="name">
</div>
```
**`[(ngModel)]`** is Angular's two-way data binding syntax.

***Angular needs to know how the pieces of your application fit together and what other files and libraries the application requires. This information is called metadata.*** **(import FormsModule)**
###Import FormsModule
app.module.ts (FormsModule symbol import) |
--- |
```
import { FormsModule } from '@angular/forms'; // <-- NgModel lives here
...
imports: [
    BrowserModule,
    FormsModule
  ],
```
