# nest-typeorm-migration-config


## create db/migrations folder in root directory
* This is where we will store all migrations.


## Create data-source.ts file in root directory, for creating data source configuration and datasource object

```
import { DataSource, DataSourceOptions } from "typeorm";

export const dataSourceOtptions : DataSourceOptions= {
    type: 'mariadb',
    host: 'localhost',
    database: 'home_nest_core_db',
    port: 3306,
    username: 'root',
    password: 'xyz',
    entities: ['dist/**/*.entity.js'],
    migrations: ['dist/db/migrations/*.js']
}

const appDataSource = new DataSource(dataSourceOtptions)
export default appDataSource;

```

## Add data source options to your root module typeorm module initializer 

```
import { Module } from '@nestjs/common';
import { TypeOrmModule } from '@nestjs/typeorm';
import { dataSourceOtptions } from 'data-source';


@Module({
  imports: [
    TypeOrmModule.forRoot(dataSourceOtptions),
  ],
})
export class AppModule {}
```

## Add these commands to package.json scripts section
```
 "typeorm" : "npm run build && npx typeorm -d dist/data-source.js",
 "migration:generate" : "npm run typeorm -- migration:generate",
 "migration:run" : "npm run typeorm -- migration:run",
 "migration:revert" : "npm run typeorm -- migration:revert"
```

## Create your entity, and finally run migrations  
- generate migration file inside db/migrations/ folder inside FileMigration file.
> npm run migration:generate -- db/migrations/FirstMigration

- apply migration, at this point your changes in database will be applied to your database
> npm run migration:run

- if you want to revert your last change, this will undo last migration.
- npm run migration:revert 
