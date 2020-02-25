# Curso de Angular 6
Angular es un framework desarrollado por Google, en este curso veremos la versión 6 aunque ya hay unas mas recientes.

## Descripción del proyecto
Vamos a desarrollar una aplicación de mensajería instantanea en la cual podremos hacer zumbidos y editar cierta información de contacto. Basicamente será un clon de messenger. 

## Instalación de ambientes
necesitamos tener instalado node.js y por medio de npm vamos a instalar angular de forma global. en este caso explicaré la instalación en ubuntu ya que es el sistema operativo que uso a la hora de tomar el curso.  
~~~sh
$ sudo apt install node
$ sudo apt install npm
$ sudo npm install -g @angular/cli
~~~

## Ruteo
Con Angular podemos hacer un ruteo de nuestros componentes usando la etiquieta ~~~<router-outlet></router-outlet>~~~ en nuestro app.component.html, y si en los enlaces en lugar de usar el href, usamos routerLink, asi nuestra página solo cargará una vez, ya que esa es la idea de una single page application.

~~~html
<a routerLink="/home">Home</a>
<router-outlet></router-outlet>
~~~

## Componentes anidados 
Podemos declarar componentes dentro de otros componentes para poder usarlos de una forma mas granualr

## TypeScript
Como bien ya sabemos, TS maneja tipos y es muy importante tenerlo en cuenta para el correcto desarrollo de nuestro proyecto.

## Interfaces en TypeScript 
Los tipos de datos Interface en TS son muy parecidos a una clase en la que se definen propiedades internas que pueden ser de cualquiera de los otros tipos. Estas propiedades internas pueden definirse como obligatorias u opcionales utilizando el simbolo "?". las interfaces definen en cierto modo estructuras personalizadas de datos en las que lo principal es que al ser implementadas usando ciertos IDEs, muestran mensajes de control y validación para asegurar el uso adecuado de dicha interface en tiempo real durante el desarrollo.  
Así se declara una interface.  
~~~ts
export interface User {
    nick: string;
    subnick?: string;
    age?: number;
    email: string;
    friend: boolean;
    uuid: any;
}
~~~

## Directivas de Angular en la vista
Existen varias directivas ng que podemos utilizar en nuestros HTML para poder interactuar con la información que le pasamos a través de nuestro .component

### ng-for
Es un ciclo que nos permite imprimir información en nuestra vista en el caso que le enviemos un listado a nuestro template, se usa de la siguiente manera:  
~~~ts
// Dentro de nuestro .component.ts
// Declaramos la variable que vamos a usar. En este caso será un arreglo de usuarios
...
  users: User[] = [];
// En el metodo ngOnInit le pasamos la data, tambien se la podriamos pasar en el constructor
  ngOnInit(): void {
    this.friends = [
      {nick: 'Eduardo', subnick: 'Mi mensaje personal', status: 'online', age: 28, email: 'eduardo@platzi.com', friend: true},
      {nick: 'Yuliana', subnick: 'Mi mensaje personal', status: 'busy', age: 25, email: 'yuliana@platzi.com', friend: true},
      {nick: 'Freddy', subnick: 'Mi mensaje personal', status: 'away', age: 28, email: 'freddy@platzi.com', friend: false}
    ];
  }
~~~  
y dentro de nuestro HTML podemos usar la directiva *ngFor para poder interactuar con la variable que le estamos pasando a través del componente e incluso declarar nuevas variables allí.  
~~~html
<p *ngFor="let friend of friends; let i = index ">
    {{ i+1 }}. {{ friend.nick }} - {{ friend.email }}
</p>
~~~

### ngIf
Es una directiva estructural de angular al igual que ngFor, nos ayuda a evaluar si una condición se cumple o no y mostrar cierto contenido en caso de ser encesario. No se pueden usar dos directivas dentro de la misma etiqueta, para ello podemos valernos de una etiqueta provista por Angular que nos permitirá contener directivas anidadas.  
~~~html
<p><b>Amigos</b></p>
<ng-container *ngFor="let friend of friends; let i = index ">
    <p *ngIf="friend.friend">
        {{ i+1 }}. {{ friend.nick }} - {{ friend.email }}
    </p>
</ng-container>

<p><b>No Amigos</b></p>
<ng-container *ngFor="let friend of friends; let i = index ">
    <p *ngIf="!friend.friend">
        {{ i+1 }}. {{ friend.nick }} - {{ friend.email }}
    </p>
</ng-container>
~~~

## Pasando parametros por la url
En Platzinger necesitamos ingresar a un chat y para ello necesitamos conocer el Id de la persona con la que queremos tener una conversación, para ello podemos pasar los parametros por la url y lo podemos hacer de la siguiente manera:  
~~~html
<!-- En nuestro HTML declaramos una <a> con el siguiente parametro -->
<p><b>Amigos</b></p>
<ng-container *ngFor="let friend of friends; let i = index ">
    <div *ngIf="friend.friend">
        <a routerLink="/conversation/{{friend.uid}}">
            {{ i+1 }}. {{ friend.nick }} - {{ friend.email }}
        </a>
    </div>
</ng-container>
~~~  
Y en nuestro app-routing ponemos la siguiente URL:  
~~~ts
const routes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'home', component: HomeComponent },
  { path: 'login', component: LoginComponent },
  /* Esta de aquí, definiendo el parametro de esta manera */
  { path: 'conversation/:uid', component: ConversationComponent },
  /* ---------------------------------------------------- */
  { path: 'profile', component: ProfileComponent },
];
~~~

## Servicios
Los servicios son archivos de TS que nos aydan a no replicar código, sino simplemente implementar funcionalidades especificas donde sea que las requiramos, los podemos crear usando el comando ~~~ng g s <<Nombre>>~~~

## Pipes en Angular
Son elementos que se pueden incluir en el HTML y nos permiten aplicar transformaciones a los datos antes de mostrarlos.  
* json
* number:'<formato decimal>'
* date:'<formato fecha>'  
Más detalles en la documentacion de [Angular-pipes](https://angular.io/guide/pipes)   
~~~html
<p>
  {{ friend | json }}
</p>
<p>
  {{ price | number: '1.0-5' }}
</p>
<p>
  {{ today | date: 'medium' }}
</p>
~~~
