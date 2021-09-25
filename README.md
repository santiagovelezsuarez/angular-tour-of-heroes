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

# Tutorial Steps
### Change the application title
app.component.ts (class title property) |
--- |
`title = 'Tour of Heroes';` |

app.component.html (template) |
--- |
`<h1>{{title}}</h1>` |

### Add application styles
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


### Create the heroes component
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
### Show the hero object
heroes.component.html (HeroesComponent's template) |
--- |
```
<h2>{{hero.name}} Details</h2>
<div><span>id: </span>{{hero.id}}</div>
<div><span>name: </span>{{hero.name}}</div>
```
### Format with the UppercasePipe
Modify the `hero.name` binding like this.
src/app/heroes/heroes.component.html |
--- |
```
<h2>{{hero.name | uppercase}} Details</h2>
```
**Pipes** are a good way to format strings, currency amounts, dates and other display data. Angular ships with several built-in pipes and you can create your own.

### Edit the hero
Users should be able to edit the hero name in an `<input>` textbox.

setup a **two-way data binding** between the `<input>` form element and the `hero.name` property.
### Two-way binding
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
### Import FormsModule
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

### Create mock heroes
For now, you'll create some mock heroes and pretend they came from the server.

src/app/mock-heroes.ts |
--- |
```
import { Hero } from './hero';

export const HEROES: Hero[] = [
  { id: 11, name: 'Dr Nice' },
  { id: 12, name: 'Narco' },
  { id: 13, name: 'Bombasto' },
  { id: 14, name: 'Celeritas' },
  { id: 15, name: 'Magneta' },
  { id: 16, name: 'RubberMan' },
  { id: 17, name: 'Dynama' },
  { id: 18, name: 'Dr IQ' },
  { id: 19, name: 'Magma' },
  { id: 20, name: 'Tornado' }
];
```
### Displaying heroes
src/app/heroes/heroes.component.ts |
--- |
```
import { HEROES } from '../mock-heroes';
...
export class HeroesComponent implements OnInit {
  
  heroes = HEROES;
}
```
heroes.component.html (heroes template) |
--- |
```
<h2>My Heroes</h2>
<ul class="heroes">
  <li *ngFor="let hero of heroes">
    <span class="badge">{{hero.id}}</span> {{hero.name}}
  </li>
</ul>
```
### Add a click event
***Binding***
heroes.component.html (heroes template) |
--- |
```
...
<li *ngFor="let hero of heroes" (click)="onSelect(hero)">
```
***event handler***
src/app/heroes/heroes.component.ts (onSelect) |
--- |
```
...
selectedHero?: Hero;
onSelect(hero: Hero): void {
  this.selectedHero = hero;
}
```

### Add a details section
To click on a hero on the list and reveal details about that hero, you need a section for the details to render in the template.

heroes.component.html (selected hero details) |
--- |
```
<div *ngIf="selectedHero">

  <h2>{{selectedHero.name | uppercase}} Details</h2>
  <div><span>id: </span>{{selectedHero.id}}</div>
  <div>
    <label for="hero-name">Hero name: </label>
    <input id="hero-name" [(ngModel)]="selectedHero.name" placeholder="name">
  </div>

</div>
```

***Angular's class binding can add and remove a CSS class conditionally. Add [class.some-css-class]="some-condition" to the element you want to style.***

Add the following `[class.selected]` binding to the `<li>` in the HeroesComponent template:
heroes.component.html (toggle the 'selected' CSS class) |
--- |
```
[class.selected]="hero === selectedHero"
```
