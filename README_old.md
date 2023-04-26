# GENARATORE PIGES

generatore di file .json di jhipster

Creare il file json di configurazione del progetto nella cartella _config
```bash
touch _config/prefix-name.yo-rc.json
```

Creare il file jdl da usare per la generazione nella cartella _config
```bash
touch _config/prefix-name.jdl
```

Creare la cartella nel quale generare i file
```bash
mkdir prefix-name
```

Entrare nella cartella creata da riga di comando
```bash
cd prefix-name
```

Eseguire la generazione con il segueente comando 
```bash
../generateApp.sh
```

Verranno quindi generati i file .json necessari al generatore di codice.

# CREAZIONE APP NESTJS

## CREARE APP

Creare il proggetto Nest
```bash
nest new back-end
cd back-end
```

Aggiungere le dipendenze per il codice generato
```bash
npm i --save @nestjs/swagger
npm i --save @nestjs/config
npm i --save @prisma/client
npm i --save class-validator
npm i --save class-transformer
npm i prisma --save-dev
```

## GENERARE IL CODICE
```bash
yo jhipster-aiggenerator
```

Importare il modulo generato ed il `ConfigModule` in `app.module.ts`
```TypeScript
@Module({
  imports: [
    ConfigModule.forRoot({
		isGlobal: true
	}),
    AssoConfratModule,
  ]
})
export class AppModule {}
```

Rinominare il file prisma generato nella cartella prisma ed aggiungere le configurazioni:
```Prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "mysql"
  url      = env("DATABASE_URL")
}
```

Creare file .env con all'interno le configurazioni del database
```
DATABASE_URL="mysql://user:password@localhost:3306/database"
```

Eseguire i comandi di generazione per prisma
```bash
npx prisma generate
npx prisma migrate dev --name init
```

Importare il database con il comando
```bash
mysql -h127.0.0.1 -uuser -ppassword database < prisma/migrations/0000_init/migration.sql
```

Lanciare l'applicazione in dev mode
```bash
npm run start:dev
```
Funziona (speriamo)!

## AGGIUNGERE SWAGGER ED ALTRE FUNZIONI NECESSARIE

Per far funzionare swagger ed altro aggiungere in main.ts
```TypeScript
async function bootstrap() {
    const app = await NestFactory.create(AppModule);

	app.useGlobalPipes(new ValidationPipe({
		whitelist: true
	}));

	app.enableCors();

    // Swagger
    {
        const config = new DocumentBuilder()
        .setTitle('Cats example')
        .setVersion('1.0')
        .build();

        const document = SwaggerModule.createDocument(app, config);
        SwaggerModule.setup('api', app, document);
    }

    const document = SwaggerModule.createDocument(app, config);
    SwaggerModule.setup('api', app, document);

    await app.listen(3000);
}
```
Per vedere la swagger doc andare sulla url:

http://localhost:3000/api

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