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

Rinominare il file prisma generato nella cartella prisma in `schema.prisma` ed aggiungere le configurazioni:
```Prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "mysql"
  url      = env("DATABASE_URL")
}
```

Creare file `.env` nella root directory con all'interno le configurazioni del database
```
BASE_URL=''
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

	app.setGlobalPrefix(process.env.BASE_URL);

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

    await app.listen(3000);
}
```
Per vedere la swagger doc andare sulla url:

http://localhost:3000/api
