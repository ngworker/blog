# Introducing @ngworker/lumberjack version 15: Improved, Modernized, and More Powerful Than Ever

Hello, Angular enthusiasts!

Today, we're thrilled to announce the release of @ngworker/lumberjack version 15. Our team has been working tirelessly to introduce some exciting changes to this OSS Angular library that you've grown to love. And trust us, this new version is _lumberjack_ in every sense: strong, agile, and efficient!

**TL;DR** - Lumberjack version 15 introduces significant updates, including all possible dependencies now being injected with the latest type-safe `inject` function. We've updated to Angular version 15, Nx version 16.1, and Node 18, and we're proud to present our standout feature: support for standalone Angular applications. Now, you can easily include Lumberjack in the `bootstrapApplication` function and configure Lumberjack drivers to suit your needs.

## Powerfully Type-Safe Dependency Injection

Firstly, we've ensured that all possible dependencies are now injected using the most recent type-safe `inject` function. This internal change not only improves the type safety of our source code but also modernizes our Angular implementations. Yes, that's right, no compromises on modernity!

## Keeping Up With The Latest

In line with our commitment to staying updated, we've made the significant leap to Angular version 15, Nx version 16.1, and Node 18. This does introduce some breaking changes, but fear not, we believe in the power of evolution!

## Standalone Angular Applications

Our most substantial addition is the support for standalone Angular applications. Now, you can simply include Lumberjack in the `bootstrapApplication` function. Here's a simple example:

```ts
bootstrapApplication(AppComponent, {
  providers: [
    // (...)
    provideLumberjack(),
    // (...)
  ],
});
```

## Configurable Drivers

We've also extended the capability to provide the Lumberjack drivers. This offers you a greater degree of control and customization. Check out the example below:

```ts
bootstrapApplication(AppComponent, {
  providers: [
    // (...)
    provideLumberjack(),
    provideLumberjackConsoleDriver(),
    provideLumberjackHttpDriver(withHttpConfig({...})),
    // (...)
  ],
});
```

## More Advanced Configuration

For those of you craving advanced configuration, we're introducing a way to configure the httpDriver, offering you even more flexibility:

```ts
import { bootstrapApplication } from "@angular/platform-browser";
import { LumberjackLevel, provideLumberjack } from "@ngworker/lumberjack";
import { provideLumberjackConsoleDriver } from "@ngworker/lumberjack/console-driver";
import {
  provideLumberjackHttpDriver,
  withHttpConfig,
} from "@ngworker/lumberjack/http-driver";
import { AppComponent } from "./app/app.component";
bootstrapApplication(AppComponent, {
  providers: [
    provideLumberjack(),
    provideLumberjackConsoleDriver(),
    provideLumberjackHttpDriver(
      withHttpConfig({
        levels: [LumberjackLevel.Critical, LumberjackLevel.Error],
        origin: "ForestApp",
        storeUrl: "/api/logs",
        retryOptions: { maxRetries: 5, delayMs: 250 },
      })
    ),
  ],
});
```

You can even configure the internal httpClient configuration:

```ts
bootstrapApplication(AppComponent, {
  providers: [
    provideLumberjack(),
    provideLumberjackConsoleDriver(),
    provideLumberjackHttpDriver(
      withHttpOptions({
        origin: 'ForestApp',
        retryOptions: { maxRetries: 1, delayMs: 250 },


 storeUrl: '/api/logs',
      }),
      withInterceptors([
        (req, next) => {
          const easy = inject(easyToken);
          console.log('are interceptors working?', easy);
          return next(req);
        },
      ])
    ),
```

## Module Support Continues

And don't worry, we still fully support modules. This new addition complements the existing features rather than replacing them.

## In Closing

We're excited about these changes and believe they will take your Angular application development to new heights. Thank you for your continued support, and as always, happy coding!
