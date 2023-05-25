# Announcing @ngworker/lumberjack version 15: A Step Forward

We are pleased to share that we've just released @ngworker/lumberjack version 15. The team have been quietly working on a few updates that we hope you will find useful. Enough small talk, let's dive into what's new!

**TL;DR** - Lumberjack version 15 introduces a few updates worth noting: internal upgrades like the use of the latest type-safe `inject` function for all possible dependencies, updates to Angular version 15, Nx version 16.1, and Node 18. The big announcement is that we've added support for standalone Angular applications. Now, you can conveniently include Lumberjack in the `bootstrapApplication` function and configure Lumberjack drivers as needed.

## Better Type Safety

First on our list, we've introduced the use of the newest type-safe `inject` function moving away from constructor paramter decorators. This is just a small step towards improving the type safety (it is quite good) of our source code and aligning our Angular implementations with recent standards.

## Keeping Current

We are all excited about everything that's happening on the JavaScript world. That's why we will continue putting efforts on keeping Lumberjack up to date with the latest version of Angular and any other tool that's part of the Lumberjack suite. This time, we have updated to Angular version 15, Nx version 16.1, and Node 18. Please note, this will introduce breaking changes; make sure to update to Angular 15 before upgrading Lumberjack and verify that your other dependecies are compatible with Node 18.

## Supporting Standalone Angular Applications

And the cherry of the cake is...

Our most exciting announcement is that we've added support standalone Angular applications. Now, you can include Lumberjack directly in the `bootstrapApplication` function. Here's a simple example of how to use the new APIs:

```ts
bootstrapApplication(AppComponent, {
  providers: [
    // (...)
    provideLumberjack(),
    // (...)
  ],
});
```

### Enhanced Driver Configuration

We've also made it possible for you to enable the standalone providers for the out-of-the-box Lumberjack drivers. Here's how you can do it:

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

### Advanced Configuration for Those Who Want More

Configuring `Lumberjack` and the `ConsoleDriver` with the new API should represent a similar experience that what we have been able to do before with Modules.

Both `Lumberjack` and the `ConsoleDriver` can be configure without any extra arguments or by passing a configuration object as the single parameter of the provide functions.

```ts
bootstrapApplication(AppComponent, {
  providers: [
    provideLumberjack({ levels: [LumberjackLevel.Error] }),
    provideLumberjackConsoleDriver({
      levels: [LumberjackLevel.Info, LumberjackLevel.Error],
    }),
  ],
});
```

The `HttpDriver` is slightly more advanced since now we need to use the `with*` config functions for the diffent configurations types.

We can use the `withHttpOptions`

```ts
bootstrapApplication(AppComponent, {
  providers: [
    provideLumberjack(),
    provideLumberjackHttpDriver(
      withHttpOptions({
        origin: 'ForestApp',
        retryOptions: { maxRetries: 1, delayMs: 250 },
        storeUrl: '/api/logs',
      }),
    ),
```

Or the `withHttpConfig`

```ts
bootstrapApplication(AppComponent, {
  providers: [
    provideLumberjack(),
    provideLumberjackHttpDriver(
      withHttpConfig({
        levels: [LumberjackLevel.Error],
        origin: 'ForestApp',
        retryOptions: { maxRetries: 1, delayMs: 250 },
        storeUrl: '/api/logs',
      }),
    ),
```

The big novelty is that now, we can also configure the underlying `HttpClient` using the second argument of the `provideLumberjackHttpDriver` function

```ts
bootstrapApplication(AppComponent, {
  providers: [
    provideLumberjack(),
    provideLumberjackHttpDriver(
      withHttpConfig({
        levels: [LumberjackLevel.Error],
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

## Continuous Module Support

We want to assure you that we continue to support modules. The new standalone APIs are a complement, not a replacement. Choose your poison.

## Wrapping Up

We hope these changes will be of help in your Angular application development journey. We're grateful for your support, and as always, happy coding!
