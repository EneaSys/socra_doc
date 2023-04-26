# CREAZIONE APP ANGULAR

## CREATE APP

For start create app with this comand:
```bash
ng new front-end
cd front-end
```
Select "Yes" to angular routing and select "scss" as stylesheet format.

Import this library:
```bash
npm i --save @angular/animations
npm i --save @enge/common-app
npm i --save @enge/common-lib
```

Da verificare

In `app.module.ts` edit the `BrowserModule` to `BrowserAnimationModule`


In `app.module.ts` import `HttpClientModule`
```TypeScript
import { BrowserAnimationsModule } from '@angular/platform-browser/animations';
import { HttpClientModule } from '@angular/common/http';
...
imports: [
    BrowserAnimationsModule,
    HttpClientModule,
    ...
]
```

In `tsconfig.json` edit the `baseUrl` and `compilerOptions` value
```Json
"compilerOptions": {
    "baseUrl": "./src/",
    "strictPropertyInitialization": false,
    ...
}
...
```

## GENERATE CODE

```bash
yo jhipster-aiggenerator
```

Point a specific route in `app-routing.module.ts` to module

```TypeScript
...
const routes: Routes = [
    ... 
    { path: 'enzo', loadChildren: () => import('./modules/enzo-prefix-name/prefix-name.module').then(m => m.EnzoPrefixNameModule) }
    ...
];
...
```

Launch the app
```bash
ng serve
```