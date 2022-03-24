## Angular Cheatsheet
BASED ON https://angular.io/guide/cheatsheet


```python
npm install --save @angular/cli       # install command line interface (CLI) for Angular apps

ng new Todo                           # Create new Todo project. Generates new Todo folder in current directory
ng serve                              # serve the app


ng g component my-new-component       # Component	MyNewComponent will be created and added to app.module.ts
ng g directive my-new-directive       # new directive
ng g pipe my-new-pipe
ng g service my-new-service


ng g class my-new-class
ng g interface my-new-interface
ng g enum my-new-enum
ng g module my-module


ng build                              # build the release
ng build --prod --base-href /aws/
ng build --prod --output-hashing none
ng build --prod --base-href /aws/ --output-path ~/Sites/aws
```


### BOOSTRAPPING
https://angular.io/guide/bootstrapping
https://angular.io/guide/ngmodules

```js
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';

// Bootstraps the app, using the root component from the specified NgModule.
platformBrowserDynamic().bootstrapModule(AppModule); 


import { NgModule } from '@angular/core';

@NgModule({
  declarations: ...,
  imports: ...,
  exports: ...,
  providers: ...,
  bootstrap: ...
})

// Defines a module that contains components, directives, pipes, and providers.
class MyModule {} 

// List of components, directives, and pipes that belong to this module.
declarations: [MyRedComponent, MyBlueComponent, MyDatePipe] 

// List of modules to import into this module. Everything from the imported modules is available to declarations of this module.
imports: [BrowserModule, SomeOtherModule] 

// List of components, directives, and pipes visible to modules that import this module.
exports: [MyRedComponent, MyDatePipe] 

// List of dependency injection providers visible both to the contents of this module and to importers of this module.
providers: [MyService, { provide: ... }]  

// List of components to bootstrap when this module is bootstrapped.
bootstrap: [MyAppComponent] 
```

### TEMPLATE SYNTAX
https://angular.io/guide/template-syntax

#### Style
```js
<!-- Binds attribute role to the result of expression myAriaRole. -->
<div [attr.role]="myAriaRole">  

<div [class.extra-sparkle]="isDelightful">  
<div [style.width.px]="mySize"> 
<i class="fa" [ngClass]="{'fa-heart-o': !item.id,'fa-heart': product.wishid}" (click)="list(item)"></i>
<div class="form-group" [ngClass]="{ 'has-error': f.submitted && !username.valid }">...</div>

// Allows you to assign styles to an HTML element using CSS.
<div [ngStyle]="{'property': 'value'}">
<div [ngStyle]="dynamicStyles()"> 
```

#### Main
```js
<input [value]="firstName"> <!-- Binds property value to the result of expression firstName -->
  
<!-- Calls method readRainbow defined in its component.ts file when a click event is triggered on this button-->
<button (click)="readRainbow($event)">  

<div title="Hello {{ponyName}}">  
<p>Hello {{ponyName}}</p> 
<p>Employer: {{employer?.companyName}}</p> <!-- If employer exists then print companyName -->

<!-- Sets up two-way data binding. Equivalent to: <my-cmp [title]="name" (titleChange)="name=$event"> -->
<my-cmp [(title)]="name"> 

<!-- Creates a local variable movieplayer that provides access to the video element instance in  data-binding and event-binding expressions in the current template. -->
<video #movieplayer ...>
  <button (click)="movieplayer.play()">
</video>  

<!-- The * symbol turns the current element into an embedded template. Equivalent to: <ng-template [myUnless]="myExpression"><p>...</p></ng-template> -->
<p *myUnless="myExpression">...</p> 

<!-- Transforms the current value of expression cardNumber via the pipe called myCardNumberFormatter. -->
<p>Card No.: {{cardNumber | myCardNumberFormatter}}</p> 
```

#### Loop
##### Table

```js
<table>
    <thead>
        <th>Name</th>
        <th>Index</th>
    </thead>
    <tbody>
        <tr *ngFor="let hero of heroes">
            <td>{{hero.name}}</td>
        </tr>
    </tbody>
</table>
```

##### Other Loop
```js
<div *ngFor="let hero of heroes; let i=index">{{i + 1}} - {{hero.fullName}}</div>  

<li *ngFor="let item of items; let i = index; trackBy: trackByFn"></li>

<select id="jarfile" (change)="sh(w)" name="jarfile" [(ngModel)]="w.jarfile">
   <option *ngFor="let t of jars" [value]="t" [attr.selected]="w.jarfile==t ? true : null">{{t}}</option>
</select>
```

##### Print Items
```js
<div *ngFor="let item of items; let firstItem = first; let lastItem = last">
  <p *ngIf="firstItem">I am the first item and I am gonna be showed</p>
  <p *ngIf="!firstItem">I am not the first item and I will not show up :(</p>
  <p *ngIf="lastItem">But I'm gonna be showed as I am the last item :)</p>
</div>
```

### BUILT-IN DIRECTIVES
https://angular.io/guide/attribute-directives

```js
import { CommonModule } from '@angular/common';

<section *ngIf="showSection"> 

<li *ngFor="let item of list">  

// Conditionally swaps the contents of the div by selecting one of the embedded templates based on 
// the current value of conditionExpression.
<div [ngSwitch]="conditionExpression">
<ng-template [ngSwitchCase]="case1Exp">...</ng-template>
<ng-template ngSwitchCase="case2LiteralString">...</ng-template>
<ng-template ngSwitchDefault>...</ng-template>
</div> 
```

### FORMS
https://angular.io/guide/forms

```js
// app.module.ts
import { FormsModule } from '@angular/forms';

@NgModule({
  declarations: [...],
  imports: [
    ...,
    FormsModule,
  ],
  providers: [...],
  bootstrap: [AppComponent]
})

// Provides two-way data-binding, parsing, and validation for form controls.
<input [(ngModel)]="userName">  
```

### CLASS DECORATORS

```js
import { Directive, ... } from '@angular/core';

@Component({...}) // Declares that a class is a component and provides metadata about the component.
class MyComponent() {}  

@Directive({...}) // Declares that a class is a directive and provides metadata about the directive.
class MyDirective() {}  

@Pipe({...})  		// Declares that a class is a pipe and provides metadata about the pipe.
class MyPipe() {} 

@Injectable()			// Declares that a class can be injected into the constructor of another class 
class MyService() {}  
```

### COMPONENT CHANGE DETECTION AND LIFECYCLE HOOKS

```js
// Called before any other lifecycle hook. Avoid any serious work here.
constructor(myService: MyService, ...) { ... }  

// Called after every change to input properties and before processing content or child views.
ngOnChanges(changeRecord) { ... } 

// Called after the constructor, initializing input properties, and the first call to ngOnChanges.
ngOnInit() { ... }  

// Called every time that the input properties of a component or a directive are checked. Use it
// to extend change detection by performing a custom check.
ngDoCheck() { ... } 

// Called after ngOnInit when the component's or directive's content has been initialized.
ngAfterContentInit() { ... }  

// Called after every check of the component's or directive's content.
ngAfterContentChecked() { ... } 

// Called after ngAfterContentInit when the component's views+child views/that a directive has been initialized.
ngAfterViewInit() { ... } 

// Called after every check of the component's views and child views / the view that a directive is in.
ngAfterViewChecked() { ... }  

// Called once, before the instance is destroyed.
ngOnDestroy() { ... } 
```

### DEPENDENCY INJECTION CONFIGURATION
https://angular.io/guide/dependency-injection

```js
// Sets or overrides the provider for MyService to the MyMockService class.
{ provide: MyService, useClass: MyMockService } 

// Sets or overrides the provider for MyService to the myFactory factory function.
{ provide: MyService, useFactory: myFactory } 

// Sets or overrides the provider for MyValue to the value 41.
{ provide: MyValue, useValue: 41 }  
```
### ROUTING AND NAVIGATION
https://angular.io/guide/router

```js
import { Routes, RouterModule, ... } from '@angular/router';

const routes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'path/:routeParam', component: MyComponent },
  { path: 'staticPath', component: ... },
  { path: '**', component: ... },
  { path: 'oldPath', redirectTo: '/staticPath' },
  { path: ..., component: ..., data: { message: 'Custom' } }
]);

// Configures routes for the application. Supports static, parameterized, redirect, and wildcard 
// routes. Also supports custom route data and resolve.
const routing = RouterModule.forRoot(routes); 

// Marks the location to load the component of the active route.
<router-outlet></router-outlet>
<router-outlet name="aux"></router-outlet>

// Creates a link to a different view based on a route instruction consisting of a route path, 
// required and optional parameters, query parameters, and a fragment. To navigate to a root 
// route, use the / prefix; for a child route, use the ./prefix; for a sibling or parent, use the 
// ../ prefix.

<a routerLink="/path">
<a [routerLink]="[ '/path', routeParam ]">
<a [routerLink]="[ '/path', { matrixParam: 'value' } ]">
<a [routerLink]="[ '/path' ]" [queryParams]="{ page: 1 }">
<a [routerLink]="[ '/path' ]" fragment="anchor">

// The provided classes are added to the element when the routerLink becomes the current active
// route.
<a [routerLink]="[ '/path' ]" routerLinkActive="active">  

class CanActivateGuard implements CanActivate {
  canActivate(
    route: ActivatedRouteSnapshot,
    state: RouterStateSnapshot
  ): Observable<boolean>|Promise<boolean>|boolean { ... }
}

// An interface for defining a class that the router should call first to determine if it should 
// activate this component. Should return a boolean or an Observable/Promise that resolves to a boolean.
{
  path: ...,
  canActivate: [CanActivateGuard]
}  

class CanDeactivateGuard implements CanDeactivate<T> {
  canDeactivate(
    component: T,
    route: ActivatedRouteSnapshot,
    state: RouterStateSnapshot
  ): Observable<boolean>|Promise<boolean>|boolean { ... }
}

// An interface for defining a class that the router should call first to determine if it should 
// deactivate this component after a navigation. Should return a boolean or an Observable/Promise 
// that resolves to a boolean.
{
  path: ...,
  canDeactivate: [CanDeactivateGuard]
}

class CanActivateChildGuard implements CanActivateChild {
  canActivateChild(
    route: ActivatedRouteSnapshot,
    state: RouterStateSnapshot
  ): Observable<boolean>|Promise<boolean>|boolean { ... }
}

// An interface for defining a class that the router should call first to determine if it should 
// activate the child route. Should return a boolean or an Observable/Promise that resolves to a  boolean.
{
  path: ...,
  canActivateChild: [CanActivateGuard],
  children: ...
}

class ResolveGuard implements Resolve<T> {
  resolve(
    route: ActivatedRouteSnapshot,
    state: RouterStateSnapshot
  ): Observable<any>|Promise<any>|any { ... }
}

// An interface for defining a class that the router should call first to resolve route data before rendering the route. Should return a value or an Observable/Promise that resolves to a value.
{
  path: ...,
  resolve: [ResolveGuard]
}

class CanLoadGuard implements CanLoad {
  canLoad(
    route: Route
  ): Observable<boolean>|Promise<boolean>|boolean { ... }
}

// An interface for defining a class that the router should call first to check if the lazy loaded module should be loaded. Should return a boolean or an Observable/Promise that resolves to a boolean.
{
  path: ...,
  canLoad: [CanLoadGuard],
  loadChildren: ...
}
```


## Input and Output

### Input

**Input()** To pass value into child component

```js
// hero-parent.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-hero-parent',
  template: `<h2>{{master}} controls {{heroes.length}} heroes</h2>
<app-hero-child *ngFor="let hero of heroes" [hero]="hero" [master]="master">
</app-hero-child>`
})
export class HeroParentComponent {
  heroes = [
    { name: 'Superman', location: 'Hong Kong' },
    { name: 'Spiderman', location: 'Dallas' },
    { name: 'Ironman', location: 'Delhi' },
  ];
  master = 'Master';
}
```

```js
// hero-child.component.ts
@Component({
  selector: 'app-hero-child',
  template: `<h3>{{hero.name}} says:</h3><p>I, {{hero.name}}, am at your service in {{hero.location}}, {{master}}.</p>`
})
export class HeroChildComponent {
  @Input() hero;
  @Input() master;
}
```

Output shows when load `<app-hero-parent>` in app.component.html

```html
Master controls 3 heroes
Superman says:
I, Superman, am at your service in Hong Kong, Master
Spiderman says:
I, Spiderman, am at your service in Dallas, Master
Ironman says:
I, Ironman, am at your service in Delhi, Master
```



### Output

**Output()** Emiting event to parent component

```js
import {Component, Input, Output, EventEmitter} from '@angular/core';

@Component({
    selector: 'yc',
    template: `<xc (notifyFromX)='onNotify($event)'></xc>`
})

export class ChildComponent {
    @Output() notifyFromY: EventEmitter<string> = new EventEmitter<string>();

    constructor(){}

    onNotify(message:string):void {
        //console.log(message, new Date());
        this.notifyFromY.emit(message + ' Y --->');
    }
}
```

### AuthGuard

```js
RouterModule.forRoot([
    ...,
    { path: 'check-out', component: CheckOutComponent, canActivate: [AuthGuard] },
    { path: 'admin/products/new', component: AdminProductEditComponent, canActivate: [AuthGuard, AdminAuthGuard] },
    { path: 'admin/products/:id', component: AdminProductEditComponent, canActivate: [AuthGuard, AdminAuthGuard] }
]);
```

```js
@Injectable({
  providedIn: 'root'
})
export class AuthGuard implements CanActivate {

  constructor(public authService: AuthService, public router: Router) {}

  canActivate(
    next: ActivatedRouteSnapshot,
    state: RouterStateSnapshot): Observable<boolean> | Promise<boolean> | boolean {
    if (this.authService.isLoggedIn !== true) {
      this.router.navigate(['login']);
    }
    return true;
  }
}
```

```js
@Injectable({
  providedIn: 'root'
})
export class AdminAuthGuard implements CanActivate {

  constructor(public authService: AuthService, public router: Router) {
  }

  canActivate(
    next: ActivatedRouteSnapshot,
    state: RouterStateSnapshot): Observable<boolean> | Promise<boolean> | boolean {

    if (this.authService.isLoggedIn !== true) {
      this.router.navigate(['login']);
    }

    if (!this.authService.isAdmin) {
      this.router.navigate(['not-found']);
    }

    return true;
  }
}
```

