# NestJs boilerplate for Kafka Consumer 

## Create from scratch
Create project
```bash
$ npm i -g @nestjs/cli
$ nest new project-name
```
Install Microservices and Kafka
```bash
npm install --save @nestjs/microservices kafkajs
```

Update the main.ts to configure Kafka Transport
```typescript
import { NestFactory } from '@nestjs/core';
import { Transport, MicroserviceOptions } from '@nestjs/microservices';

import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.createMicroservice<MicroserviceOptions>(AppModule, {
    transport: Transport.KAFKA,
    options: {
      client: {
        brokers: ['localhost:9092'],
      },
      consumer: {
          groupId: 'my-kafka-consumer',
      }
    }
  });

  app.listen(() => console.log('Kafka consumer service is listening!'))
}
bootstrap();
```
Update the app.controller.ts to listen to the topic
```typescript
import { Controller } from '@nestjs/common';
import { MessagePattern, Payload } from '@nestjs/microservices';
import { AppService } from './app.service';

@Controller()
export class AppController {

  constructor(private readonly appService: AppService) {}

  @MessagePattern('my-first-topic') // Our topic name
  getHello(@Payload() message) {
    console.log(message.value);
    return 'Hello World';
  }
}
```




## Installation

```bash
$ npm install
```

## Running the app

```bash
# development
$ npm run start

# watch mode
$ npm run start:dev

# production mode
$ npm run start:prod
```

## Test

```bash
# unit tests
$ npm run test

# e2e tests
$ npm run test:e2e

# test coverage
$ npm run test:cov
```