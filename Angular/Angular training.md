
# Introduction to Angular

## What is Angular?
Angular is a platform and framework for building single-page client applications using HTML and TypeScript. It is developed by Google and has strong tooling for developing, testing, and deploying applications.

### Key Features of Angular:
1. **Component-based Architecture**: Angular uses components as the main building blocks for creating the UI.
2. **Two-way Data Binding**: Angular provides seamless synchronization between the model and the view.
3. **Dependency Injection**: Angular's DI framework allows for easy injection of services, which makes the code cleaner and testable.
4. **Directives**: Special tokens in the markup that tell the library to do something to a DOM element (like showing, hiding, or modifying).
5. **Routing**: Angular has a robust routing module that enables navigation between different views or pages.
6. **RxJS and Observables**: Angular uses RxJS to handle asynchronous programming with events.

---

# Setting Up Angular Development Environment

### Prerequisites:
- **Node.js**: Make sure you have Node.js installed. You can download it from [Node.js](https://nodejs.org/).
- **npm**: Comes with Node.js and is used to install Angular CLI.

### Steps to Install Angular CLI:
1. Open your terminal (Command Prompt, PowerShell, or bash).
2. Run the following command to install Angular CLI globally:

    ```bash
    npm install -g @angular/cli
    ```

3. Verify the installation:

    ```bash
    ng version
    ```

4. To create a new Angular application, run:

    ```bash
    ng new my-angular-app
    ```

5. Navigate to the newly created project folder:

    ```bash
    cd my-angular-app
    ```

6. Start the development server:

    ```bash
    ng serve
    ```

7. Open your browser and navigate to `http://localhost:4200/` to see your Angular app running.

---

# Components

### Creating and Using Angular Components

Angular components are the core building blocks of an Angular application. They control the view and the behavior of a particular section of the UI.

#### Step 1: Generate a new component
To create a new component, run the following Angular CLI command:

```bash
ng generate component my-component
```

This will create a new folder with your component's files, including the TypeScript file, HTML template, CSS file, and a test spec.

#### Step 2: Using the Component
To use the new component in your app, include it in your main component (usually `app.component.html`):

```html
<app-my-component></app-my-component>
```

#### Step 3: Modifying Component Code
Inside the generated component file (`my-component.component.ts`), you can define properties and methods. For example:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-my-component',
  templateUrl: './my-component.component.html',
  styleUrls: ['./my-component.component.css']
})
export class MyComponent {
  title = 'Hello from My Component';
}
```

#### Step 4: Displaying Component Content in the Template
Inside `my-component.component.html`:

```html
<h2>{{ title }}</h2>
```

This will render "Hello from My Component" when the app runs.

# Render templates from a parent component with `ng-content`

`<ng-content>` is a special element that accepts markup or a template fragment and controls how components render content. It does not render a real DOM element.

Here is an example of a `BaseButton` component that accepts any markup from its parent.

```angular-ts
// ./base-button/base-button.component.ts
import { Component } from '@angular/core';

@Component({
  selector: 'button[baseButton]',
  standalone: true,
  template: `
      <ng-content />
  `,
})
export class BaseButton {}
```

```angular-ts
// ./app.component.ts
import { Component } from '@angular/core';
import { BaseButtonComponent } from './base-button/base-button.component.ts';

@Component({
  selector: 'app-root',
  standalone: true,
  imports: [BaseButton],
  template: `
    <button baseButton>
      Next <span class="icon arrow-right" />
    </button>
  `,
})
export class AppComponent {}
```



---

### Passing Data Between Components

#### Parent to Child Component (Using `@Input`)
You can pass data from a parent component to a child component using property binding and `@Input` decorator.

**Parent Component (app.component.html):**
```html
<app-child [childMessage]="parentMessage"></app-child>
```

**Parent Component (app.component.ts):**
```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  parentMessage = 'Message from Parent';
}
```

**Child Component (child.component.ts):**
```typescript
import { Component, Input } from '@angular/core';

@Component({
  selector: 'app-child',
  templateUrl: './child.component.html',
  styleUrls: ['./child.component.css']
})
export class ChildComponent {
  @Input() childMessage: string;
}
```

**Child Component (child.component.html):**
```html
<p>{{ childMessage }}</p>
```

This will display the message passed from the parent component.

---

### Child to Parent Component (Using `@Output` and `EventEmitter`)

To send data from a child component to a parent component, you can use the `@Output` decorator along with `EventEmitter`.

#### Step 1: Child Component (child.component.ts)

```typescript
import { Component, Output, EventEmitter } from '@angular/core';

@Component({
  selector: 'app-child',
  templateUrl: './child.component.html',
  styleUrls: ['./child.component.css']
})
export class ChildComponent {
  @Output() messageEvent = new EventEmitter<string>();

  sendMessage() {
    this.messageEvent.emit('Message from Child');
  }
}
```

#### Step 2: Child Component Template (child.component.html)

```html
<button (click)="sendMessage()">Send Message to Parent</button>
```

#### Step 3: Parent Component (app.component.html)

```html
<app-child (messageEvent)="receiveMessage($event)"></app-child>
<p>{{ message }}</p>
```

#### Step 4: Parent Component (app.component.ts)

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  message: string;

  receiveMessage($event: string) {
    this.message = $event;
  }
}
```

Now, when you click the button in the child component, the message will be sent to the parent component and displayed in the parent’s template.

---

# Service Injection for Parent-Child Communication

In Angular, services are used to share data and logic between components. Services can be injected using Angular's dependency injection mechanism.

### Step 1: Creating a Service

Generate a service using Angular CLI:

```bash
ng generate service data
```

This will create a service file (`data.service.ts`). Let's modify it to share a message between components:

**Service (data.service.ts):**

```typescript
import { Injectable } from '@angular/core';

@Injectable({
  providedIn: 'root',
})
export class DataService {
  private message: string = 'Initial Message from Service';

  setMessage(newMessage: string) {
    this.message = newMessage;
  }

  getMessage() {
    return this.message;
  }
}
```

### Step 2: Injecting Service into Parent and Child Components

Now, inject this service into the parent and child components to share data between them.

**Parent Component (app.component.ts):**

```typescript
import { Component } from '@angular/core';
import { DataService } from './data.service';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  constructor(private dataService: DataService) {}

  updateMessage() {
    this.dataService.setMessage('Updated Message from Parent');
  }

  get message() {
    return this.dataService.getMessage();
  }
}
```

**Parent Component Template (app.component.html):**

```html
<button (click)="updateMessage()">Update Message in Service</button>
<app-child></app-child>
<p>Message from Service: {{ message }}</p>
```

**Child Component (child.component.ts):**

```typescript
import { Component } from '@angular/core';
import { DataService } from '../data.service';

@Component({
  selector: 'app-child',
  templateUrl: './child.component.html',
  styleUrls: ['./child.component.css']
})
export class ChildComponent {
  constructor(private dataService: DataService) {}

  get message() {
    return this.dataService.getMessage();
  }
}
```

**Child Component Template (child.component.html):**

```html
<p>Message from Service in Child: {{ message }}</p>
```

### Explanation:

- The `DataService` is injected into both the parent and child components.
- The parent can update the message in the service using `updateMessage()`, and both the parent and child can access the updated message using the `getMessage()` method.
- This allows you to share data and logic between components without having to pass it directly through component properties.

---

# Templates and Data Binding

### Understanding Angular Templates

Angular templates are used to define the view of a component. These templates use special syntax for displaying dynamic data, iterating over lists, and handling user input.

#### Basic Example:
In `app.component.html`:

```html
<h1>Welcome to {{ title }}!</h1>
<p>The current date and time is: {{ currentDate | date:'medium' }}</p>
```

### Data Binding

Data binding in Angular connects the component's properties and methods with the UI, enabling dynamic updates.

#### 1. **Interpolation**:
Interpolation is used to display dynamic data in the template.

```html
<h1>{{ title }}</h1>
```

#### 2. **Property Binding**:
Property binding is used to bind values to HTML attributes.

```html
<img [src]="imageUrl">
```

#### 3. **Event Binding**:
Event binding is used to respond to user events like clicks.

```html
<button (click)="onClick()">Click me!</button>
```

**Component (app.component.ts):**

```typescript
import { Component

 } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'Angular App';
  imageUrl = 'https://angular.io/assets/images/logos/angular/angular.png';

  onClick() {
    alert('Button Clicked!');
  }
}
```

#### 4. **Two-way Data Binding**:
Two-way data binding in Angular allows for automatic synchronization of data between the model and the view. Changes in the UI update the model, and changes in the model automatically reflect in the UI.

To use two-way binding, you need to bind both the property and the event using `[(ngModel)]`. This is commonly used with form elements like input fields.

##### Example of Two-way Binding:

1. First, ensure that `FormsModule` is imported in your module:

```typescript
import { FormsModule } from '@angular/forms';

@NgModule({
  declarations: [ /* your components */ ],
  imports: [ BrowserModule, FormsModule ],
  bootstrap: [ AppComponent ]
})
export class AppModule { }
```

2. Then, use `[(ngModel)]` in your component's template:

**Component Template (app.component.html):**

```html
<input [(ngModel)]="name" placeholder="Enter your name">
<p>Your name is: {{ name }}</p>
```

**Component (app.component.ts):**

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  name: string = '';
}
```

#### Explanation:
- The `[(ngModel)]` syntax is used for two-way data binding, where `name` is bound to the input field.
- Any changes in the input field will update the `name` property in the component class, and any changes to `name` in the component will be automatically reflected in the input field.

---

### **`call`, `bind`, and `this` keyword**

#### **1. The `this` keyword:**
In JavaScript, the `this` keyword refers to the object it belongs to. Its value depends on the context in which it is called.

##### Example:
```typescript
class MyComponent {
  name = 'Angular';

  showName() {
    console.log(this.name);
  }
}

const component = new MyComponent();
component.showName(); // Outputs: 'Angular'
```
Here, `this.name` refers to the `name` property within the class `MyComponent`.

#### **2. The `call` Method:**
The `call` method allows you to call a function with a specified `this` value and arguments.

##### Example:
```typescript
function greet() {
  console.log(`Hello, ${this.name}`);
}

const person = { name: 'John' };
greet.call(person);  // Outputs: 'Hello, John'
```
In this example, `greet.call(person)` sets the `this` context to `person`.

#### **3. The `bind` Method:**
The `bind` method creates a new function where `this` is permanently bound to the object provided.

##### Example:
```typescript
const person = { name: 'Jane' };

function greet() {
  console.log(`Hello, ${this.name}`);
}

const boundGreet = greet.bind(person);
boundGreet();  // Outputs: 'Hello, Jane'
```
`bind` permanently sets the value of `this` to `person` for the `boundGreet` function.

Let's explore the difference between using `call`, `bind`, and `this` methods versus not using them by comparing two approaches: one with explicit context handling and one without.

### Without Using `call`, `bind`, or `this` Methods

When you do not use `call`, `bind`, or manage `this` context explicitly, the function might not behave as expected if it's invoked in different contexts.

**Example Without Context Management:**

```typescript
class Person {
  constructor(public firstName: string, public lastName: string) {}

  introduce(greeting: string) {
    console.log(`${greeting}, I am ${this.firstName} ${this.lastName}`);
  }
}

const alice = new Person('Alice', 'Johnson');
const bob = new Person('Bob', 'Smith');

// Direct invocation
alice.introduce('Hello'); // Outputs: 'Hello, I am Alice Johnson'
bob.introduce('Hi'); // Outputs: 'Hi, I am Bob Smith'

// However, if we pass the method around, context can be lost
const introduceFunction = alice.introduce;

// When called directly, `this` is not bound to the instance
introduceFunction('Greetings'); // Outputs: 'Greetings, I am undefined undefined'
```

In this case, the method `introduceFunction` loses its `this` context because it's not bound to the `alice` instance anymore. Therefore, calling it without proper context results in `undefined` for `firstName` and `lastName`.

### With Using `call`, `bind`, or `this` Methods

Using `call`, `bind`, or correctly handling `this` ensures that the method behaves as expected regardless of how it's invoked.

**Example With Context Management:**

```typescript
class Person {
  constructor(public firstName: string, public lastName: string) {}

  introduce(greeting: string) {
    console.log(`${greeting}, I am ${this.firstName} ${this.lastName}`);
  }
}

const alice = new Person('Alice', 'Johnson');
const bob = new Person('Bob', 'Smith');

// Using bind to create a new function with a fixed `this` context
const introduceAlice = alice.introduce.bind(alice, 'Hello');
const introduceBob = bob.introduce.bind(bob, 'Hi');

// Calls with bound context
introduceAlice(); // Outputs: 'Hello, I am Alice Johnson'
introduceBob(); // Outputs: 'Hi, I am Bob Smith'

// Using call to invoke the method with different context
const introduceBobUsingCall = alice.introduce.call(bob, 'Greetings');
introduceBobUsingCall(); // Outputs: 'Greetings, I am Bob Smith'

// The direct function reference with call
const introduceFunction = alice.introduce;
introduceFunction.call(alice, 'Hey'); // Outputs: 'Hey, I am Alice Johnson'
```

In this example:

- **`bind`** is used to create new functions (`introduceAlice` and `introduceBob`) with `this` pre-bound to specific instances. These functions work correctly regardless of how they are called.
- **`call`** is used to invoke `alice.introduce` with `this` bound to `bob`, allowing the method to run with `bob`'s context.
- The direct function reference with `call` (`introduceFunction.call(alice, 'Hey')`) ensures the method retains the correct context.

By using `call`, `bind`, or properly managing `this`, you maintain control over the context in which methods are executed, avoiding issues that arise from losing `this` reference.

---

### **Directives**

Directives are instructions in the DOM that tell Angular how to render or manipulate elements.

#### **1. Using Built-in Directives**

##### **`ngIf`:**
The `ngIf` directive conditionally includes a template based on a boolean value.

```html
<div *ngIf="isVisible">
  This text is visible if isVisible is true.
</div>
```

**Component (app.component.ts):**

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  isVisible = true;
}
```

Here, the `<div>` will only be rendered if `isVisible` is `true`.

##### **`ngFor`:**
The `ngFor` directive repeats a portion of the template for each item in a list.

```html
<ul>
  <li *ngFor="let item of items">{{ item }}</li>
</ul>
```

**Component (app.component.ts):**

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  items = ['Item 1', 'Item 2', 'Item 3'];
}
```

This will render a list of items as `<li>` elements.

---

#### **2. Creating Custom Directives**

Custom directives allow you to create reusable behavior for elements. Here's an example of creating a simple custom directive to change the background color of an element when hovered.

##### **Step 1: Generate a Custom Directive**

Generate a directive using Angular CLI:

```bash
ng generate directive highlight
```

##### **Step 2: Implement the Custom Directive**

**Custom Directive (highlight.directive.ts):**

```typescript
import { Directive, ElementRef, HostListener } from '@angular/core';

@Directive({
  selector: '[appHighlight]'
})
export class HighlightDirective {
  constructor(private el: ElementRef) {}

  @HostListener('mouseenter') onMouseEnter() {
    this.highlight('yellow');
  }

  @HostListener('mouseleave') onMouseLeave() {
    this.highlight(null);
  }

  private highlight(color: string) {
    this.el.nativeElement.style.backgroundColor = color;
  }
}
```

This directive changes the background color of the host element when the mouse enters and resets it when the mouse leaves.

##### **Step 3: Use the Custom Directive**

Apply the directive in your template:

**Template (app.component.html):**

```html
<p appHighlight>Hover over this text to see the highlight effect!</p>
```

Now, when you hover over the paragraph, the background will change to yellow, and it will reset when the mouse leaves.


# Angular Advanced Topics

## Custom Pipes

Pipes in Angular are used for transforming data before displaying it in the view. Angular provides several built-in pipes, but you can also create custom pipes for more specific data transformations.

### Creating Custom Pipes

1. **Generate a Custom Pipe:**

   Use Angular CLI to generate a new pipe:

   ```bash
   ng generate pipe myCustomPipe
   ```

2. **Implement the Custom Pipe:**

   Update the generated pipe file (`my-custom-pipe.pipe.ts`):

   ```typescript
   import { Pipe, PipeTransform } from '@angular/core';

   @Pipe({
     name: 'myCustomPipe'
   })
   export class MyCustomPipePipe implements PipeTransform {
     transform(value: string, ...args: unknown[]): string {
       // Example transformation: convert to uppercase
       return value.toUpperCase();
     }
   }
   ```

3. **Use the Custom Pipe:**

   Apply the custom pipe in your component's template:

   **Template (app.component.html):**

   ```html
   <p>{{ 'hello world' | myCustomPipe }}</p>  <!-- Outputs: HELLO WORLD -->
   ```

   This will transform "hello world" to uppercase using `myCustomPipe`.

---

## Services and Dependency Injection

### Creating and Using Services

1. **Generate a Service:**

   Use Angular CLI to generate a new service:

   ```bash
   ng generate service myService
   ```

2. **Implement the Service:**

   Update the generated service file (`my-service.service.ts`):

   ```typescript
   import { Injectable } from '@angular/core';

   @Injectable({
     providedIn: 'root',
   })
   export class MyService {
     private message: string = 'Hello from MyService';

     getMessage(): string {
       return this.message;
     }
   }
   ```

3. **Inject and Use the Service in a Component:**

   **Component (app.component.ts):**

   ```typescript
   import { Component } from '@angular/core';
   import { MyService } from './my-service.service';

   @Component({
     selector: 'app-root',
     templateUrl: './app.component.html',
     styleUrls: ['./app.component.css']
   })
   export class AppComponent {
     message: string;

     constructor(private myService: MyService) {
       this.message = this.myService.getMessage();
     }
   }
   ```

   **Template (app.component.html):**

   ```html
   <p>{{ message }}</p>
   ```

### Dependency Injection in Angular

Dependency injection (DI) is a design pattern used to implement IoC (Inversion of Control). Angular's DI system allows you to inject services or other dependencies into components or other services.

1. **Provide Services:**

   Services can be provided at the root level or in a specific module/component. The `@Injectable` decorator with `providedIn: 'root'` makes the service available application-wide.

   ```typescript
   @Injectable({
     providedIn: 'root',
   })
   export class MyService {
     // Service implementation
   }
   ```

2. **Inject Services:**

   Services are injected via constructor injection in components or other services.

   ```typescript
   constructor(private myService: MyService) {
     // Use the service
   }
   ```

---

## Routing

Routing in Angular allows you to navigate between different views or pages in your application.

### Setting Up Routing

1. **Generate Components for Routing:**

   Use Angular CLI to generate components for your routes:

   ```bash
   ng generate component home
   ng generate component about
   ```

2. **Configure Routes:**

   Define routes in your application's routing module (`app-routing.module.ts`):

   ```typescript
   import { NgModule } from '@angular/core';
   import { RouterModule, Routes } from '@angular/router';
   import { HomeComponent } from './home/home.component';
   import { AboutComponent } from './about/about.component';

   const routes: Routes = [
     { path: '', component: HomeComponent },
     { path: 'about', component: AboutComponent },
   ];

   @NgModule({
     imports: [RouterModule.forRoot(routes)],
     exports: [RouterModule]
   })
   export class AppRoutingModule { }
   ```

3. **Add Router Outlet:**

   Include `<router-outlet>` in your root component's template (`app.component.html`):

   ```html
   <router-outlet></router-outlet>
   ```

   This is where routed components will be displayed.

### Creating Navigation Menus

1. **Add Navigation Links:**

   Define navigation links using `routerLink` in your component template:

   **Template (app.component.html):**

   ```html
   <nav>
     <a routerLink="/">Home</a>
     <a routerLink="/about">About</a>
   </nav>
   <router-outlet></router-outlet>
   ```

   Clicking on these links will navigate to the corresponding components.

---

## HTTP Client

Angular's `HttpClient` service allows you to make HTTP requests to a server and consume RESTful APIs.

### Making HTTP Requests

1. **Import `HttpClientModule`:**

   Import `HttpClientModule` in your module file (`app.module.ts`):

   ```typescript
   import { HttpClientModule } from '@angular/common/http';

   @NgModule({
     imports: [ HttpClientModule, /* other modules */ ],
     // other configurations
   })
   export class AppModule { }
   ```

2. **Generate a Service for HTTP Requests:**

   Use Angular CLI to generate a service for making HTTP requests:

   ```bash
   ng generate service data
   ```

3. **Implement HTTP Requests in the Service:**

   Update the generated service file (`data.service.ts`):

   ```typescript
   import { Injectable } from '@angular/core';
   import { HttpClient } from '@angular/common/http';
   import { Observable } from 'rxjs';

   @Injectable({
     providedIn: 'root'
   })
   export class DataService {
     private apiUrl = 'https://api.example.com/data';

     constructor(private http: HttpClient) {}

     getData(): Observable<any> {
       return this.http.get<any>(this.apiUrl);
     }
   }
   ```

4. **Consume the Service in a Component:**

   **Component (app.component.ts):**

   ```typescript
   import { Component, OnInit } from '@angular/core';
   import { DataService } from './data.service';

   @Component({
     selector: 'app-root',
     templateUrl: './app.component.html',
     styleUrls: ['./app.component.css']
   })
   export class AppComponent implements OnInit {
     data: any;

     constructor(private dataService: DataService) {}

     ngOnInit() {
       this.dataService.getData().subscribe(response => {
         this.data = response;
       });
     }
   }
   ```

   **Template (app.component.html):**

   ```html
   <div *ngIf="data">
     <pre>{{ data | json }}</pre>
   </div>
   ```

   This will display the data fetched from the API in JSON format.

