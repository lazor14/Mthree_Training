# SRE TRAINING March 12 - ANGULAR

## Introduction to Angular
Angular is a TypeScript-based open-source front-end web application framework developed by Google. It is used to build single-page applications (SPAs) and dynamic web applications with a component-based architecture.

## Key Features of Angular
- **Component-based architecture**: Applications are built using reusable components.
- **Two-way data binding**: Synchronizes data between the model and the view.
- **Dependency injection**: Provides a modular approach to managing dependencies.
- **Directives**: Enhance HTML with additional functionalities.
- **Routing**: Enables navigation between different views in an application.
- **Built-in HTTP client**: Fetches data from APIs.
- **Standalone components (New Feature)**: Allows building Angular applications without traditional modules.

## Understanding Standalone Components
Angular introduced **Standalone Components** to simplify application structure by eliminating the need for `NgModules`. Previously, every component had to be declared inside a module (`app.module.ts`). With standalone components, each component is self-contained and can import only the necessary dependencies.

### Benefits of Standalone Components
✅ **No need for `app.module.ts`**, reducing boilerplate code.
✅ **Faster startup time**, as fewer dependencies need to be loaded.
✅ **More modular and reusable**, making it easier to manage components across projects.
✅ **Direct component imports**, simplifying dependency management.

### Example of a Standalone Component
*In an ideal project, an app will come in a default standalone mode, as it is the latest feature of Angular*
```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-home',
  standalone: true,
  template: `<h2>Welcome to Home</h2>`
})
export class HomeComponent {}
```

The main `app.component.ts` file imports and uses the standalone components directly:
```typescript
import { Component } from '@angular/core';
import { HomeComponent } from './home/home.component';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [HomeComponent],
  template: `
    <h1>My Angular App</h1>
    <app-home></app-home>
  `,
})
export class AppComponent {}
```
*To disable the standalone feature, use the --no-standalone flag while creating the new app.*

## Different Routing Methods in Angular
### **1. RouterLink**
- Used in templates to navigate between routes without reloading the page.
- Example:
  ```html
  <a routerLink="/about">Go to About</a>
  ```
- It updates the URL and Angular handles navigation internally.

### **2. RouterOutlet**
- A placeholder in the template where the routed component will be displayed.
- Example:
  ```html
  <router-outlet></router-outlet>
  ```
- This ensures that the component mapped to the current route gets rendered in place of the `<router-outlet>`.

### **3. Path-based Routing in Routing Module**
- Defines routes and maps them to components.
- Example:
  ```typescript
  { path: 'contact', component: ContactComponent }
  ```
- When the user navigates to `/contact`, Angular loads `ContactComponent` in the `<router-outlet>`.

### When to Use Each Routing Method
| Routing Method | When to Use |
|---------------|------------|
| **RouterLink** | When you need navigation links in templates without reloading the page. |
| **RouterOutlet** | When you need to display components dynamically based on the current route. |
| **Path-based Routing** | When defining navigation structure in the routing configuration. |

## Step-by-Step Project Implementation
### 1. Creating a New Angular Project
We created a new Angular project named `my-angular-app` using the Angular CLI:
```sh
ng new my-angular-app
```

### 2. Creating Components
We created three additional components for navigation:
```sh
ng generate component home
ng generate component about
ng generate component contact
```

### 3. Adding Routing
We configured routing in `app.routes.ts` to enable navigation between these components:
```typescript
import { Routes } from '@angular/router';
import { HomeComponent } from './home/home.component';
import { AboutComponent } from './about/about.component';
import { ContactComponent } from './contact/contact.component';

export const routes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'about', component: AboutComponent },
  { path: 'contact', component: ContactComponent }
];
```

### 4. Modifying Component Content
We updated each component's content to display relevant information:

**`home.component.html`**
```html
<h2>Welcome to the Home Page</h2>
<p>This is the main landing page of our Angular application.</p>
```

**`about.component.html`**
```html
<h2>About Us</h2>
<p>Learn more about our team and mission.</p>
```

**`contact.component.html`**
```html
<h2>Contact Us</h2>
<p>Get in touch with us through email or phone.</p>
```
### 5. Modifying app.component.ts file
We updated the main app.component.ts file to reflect the components.

**`app.component.ts`**
```html
<h1>My Angular App</h1>
<nav>
  <a routerLink="/home">Home</a> |
  <a routerLink="/about">About</a> |
  <a routerLink="/contact">Contact</a>
</nav>
<hr>
<router-outlet></router-outlet>
```
### 6. Running the Application
We used the following command to start the Angular application:
```sh
ng serve
```
**CLI Output** : ![ng-serve](https://github.com/rhearobinson19/Mthree_Daily_Documentation/blob/main/WEEK%205/SRE%20TRAINING%20(DAY%2023)%20-%20ANGULAR/ng_serve.png)
This command starts the development server and makes the application accessible at `http://localhost:4200/`.
<br> <br>
**Broswer Output** : <br> ![Browser](https://github.com/rhearobinson19/Mthree_Daily_Documentation/blob/main/WEEK%205/SRE%20TRAINING%20(DAY%2023)%20-%20ANGULAR/output.png)


## Conclusion
In this session, we:
- Created a new Angular project.
- Implemented **routing** with `app.routes.ts`.
- Learned about **Standalone Components**, which remove the need for `app.module.ts` and simplify Angular development.
- Understood the **differences between RouterLink, RouterOutlet, and Path-based Routing**.
- Created **three components** (Home, About, Contact) and updated their content.
- Successfully ran the Angular application using `ng serve`.


