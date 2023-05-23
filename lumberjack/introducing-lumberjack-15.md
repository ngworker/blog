# Announcing @ngworker/lumberjack version 15: A Step Forward

We are pleased to share that we've just released @ngworker/lumberjack version 15. The team have been quietly working on a few updates that we hope you will find useful. Enough small talk, let's dive into what's new!

**TL;DR** - Lumberjack version 15 introduces a few updates worth noting: internal upgrades like the use of the latest type-safe `inject` function for all possible dependencies, updates to Angular version 15, Nx version 16.1, and Node 18. The big announcement is that we've added support for standalone Angular applications. Now, you can conveniently include Lumberjack in the `bootstrapApplication` function and configure Lumberjack drivers as needed.

## Better Type Safety

First on our list, we've introduced the use of the newest type-safe `inject` function for injecting all possible dependencies. This is just a small step towards improving the type safety of our source code and aligning our Angular implementations with recent standards.

## Keeping Current

We believe it's important to stay up-to-date. With that in mind, we've updated to Angular version 15, Nx version 16.1, and Node 18. Please note, this will introduce some breaking changes, but we trust you'll navigate them like the pros you are!

## Supporting Standalone Angular Applications

A significant change in this version is the added support for standalone Angular applications. Now, you can include Lumberjack directly in the `bootstrapApplication` function. Here's a simple example:

```ts
bootstrapApplication(AppComponent, {
  providers: [
    // (...)
    provideLumberjack(),
    // (...)
  ],
});
```

## Enhanced Driver Configuration

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

## Advanced Configuration for Those Who Want More

Configuring `Lumberjack` and the `ConsoleDriver` with the new API should be a familiar compared to how we configure the modules.

It is as simple as passing the configuration object as the single parameter of the provide functions.

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

Or the `withHttpOptions`

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

We want to assure you that we continue to support modules. The new standalone APIs are a complement, not a replacement.

## Wrapping Up

We hope these changes will be of help in your Angular application development journey. We're grateful for your support, and as always, happy coding!
