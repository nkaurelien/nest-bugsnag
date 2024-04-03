<p align="center">
  <a href="http://nestjs.com/" target="blank"><img src="https://nestjs.com/img/logo_text.svg" width="320" alt="Nest Logo" /></a>
</p>

 <p align="center">A Nest module wrapper for bugsnag logger.</p>
 
<p align="center">

<a href="https://www.npmjs.com/package/@nkaurelien/nest-bugsnag"><img src="https://img.shields.io/npm/v/@nkaurelien/nest-bugsnag.svg" alt="NPM Version" /></a>
<a href="https://github.com/nkaurelien/nest-bugsnag"><img src="https://img.shields.io/npm/l/@nkaurelien/nest-bugsnag.svg" alt="Package License" /></a>
<a href="https://twitter.com/AstridNkumbe"><img src="https://img.shields.io/twitter/follow/AstridNkumbe.svg?style=social&label=Follow"></a>

</p>

## Description

A [Nest](https://github.com/nestjs/nest) module wrapper for [bugsnag-js](https://github.com/bugsnag/bugsnag-js) logger.

## Installation

```bash
$ npm i @bugsnag/plugin-express --save
$ npm i @nkaurelien/nest-bugsnag --save
```

## Quick Start

Import the `BugsnagModule` into the module. For example `AppModule`:

```typescript
import { Module } from '@nestjs/common';
import { BugsnagModule } from '@nkaurelien/nest-bugsnag';
import BugsnagPluginExpress from '@bugsnag/plugin-express'


@Module({
  imports: [
    BugsnagModule.forRoot({
          apiKey: '<API_KEY>',
          plugins: [BugsnagPluginExpress],
      }),
  ],
})
export class AppModule { }
```

In the **main.ts** file, change the HTTP platform to use express

```typescript
// change
const app = await NestFactory.create(AppModule);
// to
const app = await NestFactory.create<NestExpressApplication>(AppModule);
```

This handles any errors that Express catches  

```typescript
app.get(BugsnagService).handleAnyErrors(app);
```

Then you can inject BugsnagService. Example:

```typescript
import { Controller } from '@nestjs/common';
import { BugsnagService } from '@nkaurelien/nest-bugsnag';

@Controller('cats')
export class CatsController {
  constructor(private readonly logger: BugsnagService) { }
}
```

BugsnagService has instance property which wrap bugsnag client. So you can access it by calling:

```typescript
try {
  something.risky()
} catch (e) {
    this.logger.instance.notify('message');
}
```
 In your controller, you can call req.bugsnag.notify(err)  which will include information about the request in the error report. For example:
 
```typescript
 @Get()
 getHello(@Request() req): string {
     req.bugsnag.notify(
         new Error('First Error'),
         function (event) {
             // event.addMetadata('product', product)
         });
     return 'Hello World!';
 }
```
Note that `BugsnagModule` is a global module, it will be available in all you feature modules.


## Async configuration Sample

```typescript
import { Module } from '@nestjs/common';
import { BugsnagModule } from '@nkaurelien/nest-bugsnag';

@Module({
  imports: [
    BugsnagModule.forRootAsync({
      useFactory: (configService: ConfigService) => ({
        // options
      }),
      inject: [ConfigService],
    }),
  ],
})
export class AppModule { }
```

The factory might be async and is able to inject dependencies through the `inject` option.


## Keywords
bugsnagJs, nestJs, logger


# Donate
[![Buy Me A Coffee](https://www.buymeacoffee.com/assets/img/custom_images/orange_img.png)](https://www.paypal.com/donate/?hosted_button_id=FSXZJUZCHWG5N)

