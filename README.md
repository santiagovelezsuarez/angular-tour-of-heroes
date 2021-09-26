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
(https://angular.io/tutorial/toh-pt1)
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

### Create a feature component

At the moment, the HeroesComponent displays both the list of heroes and the selected hero's details.

Keeping all features in one component as the application grows will not be maintainable. You'll want to split up large components into smaller sub-components, each focused on a specific task or workflow.

- Use the Angular CLI to generate a new component named hero-detail.
  `$ ng generate component hero-detail`

src/app/hero-detail/hero-detail.component.html |
--- |
```
<div *ngIf="hero">

  <h2>{{hero.name | uppercase}} Details</h2>
  <div><span>id: </span>{{hero.id}}</div>
  <div>
    <label for="hero-name">Hero name: </label>
    <input id="hero-name" [(ngModel)]="hero.name" placeholder="name">
  </div>

</div>
```
### Add the @Input() hero property
The hero property must be an Input property, annotated with the @Input() decorator, because the external HeroesComponent will bind to it like this.
`<app-hero-detail [hero]="selectedHero"></app-hero-detail>`
src/app/hero-detail/hero-detail.component.ts |
--- |
```
import { Component, OnInit, Input } from '@angular/core';
import { Hero } from '../hero';
...
@Input() hero?: Hero;
```
*This component only receives a hero object through its hero property and displays it.*

### Update the HeroesComponent template
heroes.component.html + (HeroDetail binding) |
--- |
```
<h2>My Heroes</h2>

<ul class="heroes">
  <li *ngFor="let hero of heroes"
    [class.selected]="hero === selectedHero"
    (click)="onSelect(hero)">
    <span class="badge">{{hero.id}}</span> {{hero.name}}
  </li>
</ul>

<app-hero-detail [hero]="selectedHero"></app-hero-detail>
```
*The browser refreshes and the application starts working again as it did before.*

### Add services
The Tour of Heroes HeroesComponent is currently getting and displaying fake data.

After the refactoring in this tutorial, HeroesComponent will be lean and focused on supporting the view. It will also be easier to unit-test with a mock service.

Services are a great way to share information among classes that don't know each other. You'll create a ***MessageService*** and inject it in two places.

`$ ng generate service hero`

*Notice that the new service imports the Angular Injectable symbol and annotates the class with the **@Injectable()** decorator. This marks the class as one that participates in the dependency injection system.*

### Get hero data
The HeroService could get hero data from anywhere—a web service, local storage, or a mock data source.

src/app/hero.service.ts |
--- |
```
import { Injectable } from '@angular/core';
import { Hero } from './hero';
import { HEROES } from './mock-heroes';

@Injectable({
  providedIn: 'root'
})
export class HeroService {

  constructor() { }

  getHeroes(): Hero[] {
    return HEROES;
  }

}
```

**Provide the HeroService**
You must make the HeroService available to the dependency injection system before Angular can inject it into the HeroesComponent by registering a provider. A provider is something that can create or deliver a service; in this case, it instantiates the HeroService class to provide the service.
```
@Injectable({
  providedIn: 'root',
})
```
When you provide the service at the **root** level, Angular creates a single, shared instance of `HeroService` and injects into any class that asks for it.
***The HeroService is now ready to plug into the HeroesComponent.***

### Update HeroesComponent
src/app/heroes/heroes.component.ts |
--- |
```
...
import { HeroService } from '../hero.service';
...
heroes: Hero[] = [];
constructor(private heroService: HeroService) {}
```
The parameter simultaneously defines a private `heroService` property and identifies it as a `HeroService` injection site.

When Angular creates a `HeroesComponent`, the Dependency Injection system sets the `heroService` parameter to the singleton instance of `HeroService`.

***Create a method to retrieve the heroes from the service and Call it in ngOnInit()***
src/app/heroes/heroes.component.ts |
--- |
```
...
ngOnInit() {
  this.getHeroes();
}
...
getHeroes(): void {
  this.heroes = this.heroService.getHeroes();
}
```
*After the browser refreshes, the application should run as before,*

This will not work in a real application(The HeroService.getHeroes() method has a synchronous signature). You're getting away with it now because the service currently returns mock heroes. But soon the application will fetch heroes from a remote server, which is an inherently asynchronous operation.

The `HeroService` must wait for the server to respond, `getHeroes()` cannot return immediately with hero data, and the browser will not block while the service waits.

`HeroService.getHeroes()` must have an asynchronous signature of some kind.

In this tutorial, `HeroService.getHeroes()` will return an `Observable` because it will eventually use the Angular `HttpClient.get` method to fetch the heroes and `HttpClient.get()` returns an `Observable`.

`Observable` is one of the key classes in the RxJS library.

`of(HEROES)` returns an `Observable<Hero[]>` that emits a single value, the array of mock heroes.
src/app/hero.service.ts |
--- |
```
import { Observable, of } from 'rxjs';
...
getHeroes(): Observable<Hero[]> {
  const heroes = of(HEROES);
  return heroes;
}
```
***Subscribe in HeroesComponent***
The `HeroService.getHeroes` method used to return a `Hero[]`. Now it returns an `Observable<Hero[]>`.

You'll have to adjust to that difference in `HeroesComponent`.
heroes.component.ts  |
--- |
```
...
getHeroes(): void {
  this.heroService.getHeroes()
      .subscribe(heroes => this.heroes = heroes);
}
```
***This asynchronous approach will work when the HeroService requests heroes from the server.***

### Show messages
create the MessagesComponent.
`$ ng generate component messages`

Modify the `AppComponent` template
src/app/app.component.html  |
--- |
```
<h1>{{title}}</h1>
<app-heroes></app-heroes>
<app-messages></app-messages>
```
### Create the MessageService
`$ ng generate service message`
src/app/message.service.ts  |
--- |
```
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root',
})
export class MessageService {
  messages: string[] = [];

  add(message: string) {
    this.messages.push(message);
  }

  clear() {
    this.messages = [];
  }
}
```
**Inject it into the HeroService**
Angular will inject the singleton `MessageService` into that property when it creates the `HeroService`.
src/app/hero.service.ts |
--- |
```
...
import { MessageService } from './message.service';
...
...
constructor(private messageService: MessageService) { }
```
**Send a message from HeroService**
Modify the getHeroes() method to send a message when the heroes are fetched.
src/app/hero.service.ts |
--- |
```
getHeroes(): Observable<Hero[]> {
  const heroes = of(HEROES);
  this.messageService.add('HeroService: fetched heroes');
  return heroes;
}
```
### Display the message from HeroService
The MessagesComponent should display all messages, including the message sent by the HeroService when it fetches heroes.
src/app/messages/messages.component.ts |
--- |
```
...
import { MessageService } from '../message.service';
...
constructor(public messageService: MessageService) {}
```
The messageService property must be **public** because you're going to bind to it in the template.(Angular only binds to public component properties.)

### Bind to the MessageService
src/app/messages/messages.component.html |
--- |
```
<div *ngIf="messageService.messages.length">

  <h2>Messages</h2>
  <button class="clear"
          (click)="messageService.clear()">Clear messages</button>
  <div *ngFor='let message of messageService.messages'> {{message}} </div>

</div>
```
### Add additional messages to hero service
The following example shows how to send and display a message each time the user clicks on a hero, showing a history of the user's selections. 
src/app/heroes/heroes.component.ts |
--- |
```
import { Component, OnInit } from '@angular/core';

import { Hero } from '../hero';
import { HeroService } from '../hero.service';
import { MessageService } from '../message.service';

@Component({
  selector: 'app-heroes',
  templateUrl: './heroes.component.html',
  styleUrls: ['./heroes.component.css']
})
export class HeroesComponent implements OnInit {

  selectedHero?: Hero;

  heroes: Hero[] = [];

  constructor(private heroService: HeroService, private messageService: MessageService) { }

  ngOnInit() {
    this.getHeroes();
  }

  onSelect(hero: Hero): void {
    this.selectedHero = hero;
    this.messageService.add(`HeroesComponent: Selected hero id=${hero.id}`);
  }

  getHeroes(): void {
    this.heroService.getHeroes()
        .subscribe(heroes => this.heroes = heroes);
  }
}
```

### Add navigation with routing

- Add a Dashboard view.
- Add the ability to navigate between the Heroes and Dashboard views.
- When users click a hero name in either view, navigate to a detail view of the selected hero.
- When users click a deep link in an email, open the detail view for a particular hero.

### Add the AppRoutingModule
`$ ng generate module app-routing --flat --module=app`
```
--flat puts the file in src/app instead of its own folder.
--module=app tells the CLI to register it in the imports array of the AppModule.
```

src/app/app-routing.module.ts (updated) |
--- |
```
import { NgModule } from '@angular/core';
import { RouterModule, Routes } from '@angular/router';
import { HeroesComponent } from './heroes/heroes.component';

const routes: Routes = [
  { path: 'heroes', component: HeroesComponent }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```
***
A typical Angular Route has two properties:

- path: a string that matches the URL in the browser address bar.
- component: the component that the router should create when navigating to this route.
```
const routes: Routes = [
  { path: 'heroes', component: HeroesComponent }
];
```
***

### Add RouterOutlet
src/app/app.component.html (router-outlet) |
--- |
```
<h1>{{title}}</h1>
<router-outlet></router-outlet>
<app-messages></app-messages>
```
The `AppComponent` template no longer needs `<app-heroes>` because the application will only display the `HeroesComponent` when the user navigates to it.

### Add a navigation link (routerLink)
Add a `<nav>` element and, within that, an anchor element that, when clicked, triggers navigation to the `HeroesComponent`.
src/app/app.component.html (heroes RouterLink) |
--- |
```
<h1>{{title}}</h1>
<nav>
  <a routerLink="/heroes">Heroes</a>
</nav>
<router-outlet></router-outlet>
<app-messages></app-messages>
```

### Add a dashboard view
Add a `DashboardComponent` using the CLI:
`$ ng generate component dashboard`
src/app/dashboard/dashboard.component.html |
--- |
```
<h2>Top Heroes</h2>
<div class="heroes-menu">
  <a *ngFor="let hero of heroes">
    {{hero.name}}
  </a>
</div>
```
src/app/dashboard/dashboard.component.ts |
--- |
```
import { Component, OnInit } from '@angular/core';
import { Hero } from '../hero';
import { HeroService } from '../hero.service';

@Component({
  selector: 'app-dashboard',
  templateUrl: './dashboard.component.html',
  styleUrls: [ './dashboard.component.css' ]
})
export class DashboardComponent implements OnInit {
  heroes: Hero[] = [];

  constructor(private heroService: HeroService) { }

  ngOnInit() {
    this.getHeroes();
  }

  getHeroes(): void {
    this.heroService.getHeroes()
      .subscribe(heroes => this.heroes = heroes.slice(1, 5));
      //returning only four of the Top Heroes (2nd, 3rd, 4th, and 5th).
  }
}
```
### Add the dashboard route
To navigate to the dashboard, the router needs an appropriate route.

src/app/app-routing.module.ts |
--- |
```
...
import { DashboardComponent } from './dashboard/dashboard.component';
...
{ path: 'dashboard', component: DashboardComponent },

```

### Add a default route
`{ path: '', redirectTo: '/dashboard', pathMatch: 'full' },`
This route redirects a URL that fully matches the empty path to the route whose path is `'/dashboard'`

### Add dashboard link to the shell
The user should be able to navigate back and forth between the `DashboardComponent` and the `HeroesComponent` by clicking links in the navigation area near the top of the page.

src/app/app.component.html |
--- |
```
<h1>{{title}}</h1>
<nav>
  <a routerLink="/dashboard">Dashboard</a>
  <a routerLink="/heroes">Heroes</a>
</nav>
<router-outlet></router-outlet>
<app-messages></app-messages>
```

### Navigating to hero details
- By clicking a hero in the dashboard.
- By clicking a hero in the heroes list.
- By pasting a "deep link" URL into the browser address bar that identifies the hero to display.

https://angular.io/tutorial/toh-pt5
