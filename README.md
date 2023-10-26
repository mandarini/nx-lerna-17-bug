# Angular lib generator issue with directory + Lerna

Using as directory the directory specified in `packages` property in `lerna.json` when generating an Angular library does not work.

Example, in `lerna.json` you have `"packages": ["libs/*"],`, then `--directory=libs/<ANYTHING>` does not work.

Error starts happening on Nx 17. Same lerna config works with Nx 16.10.

## Steps to reproduce

1. Install dependencies `npm i`
2. Generate an Angular library:

```bash
nx g @nx/angular:library --name=my-lib --buildable=true --publishable=true --importPath=@myorg/my-lib  --directory=libs/my-lib  --projectNameAndRootFormat=as-provided --dry-run
```

This works because we're on Nx 16.10.

## Nx 17.0.0

```bash
git checkout migrate-17
```

and install dependencies

```bash
npm i
```

Now try running the same command:

```bash
nx g @nx/angular:library --name=my-lib --buildable=true --publishable=true --importPath=@myorg/my-lib  --directory=libs/my-lib  --projectNameAndRootFormat=as-provided --dry-run
```

You get the error:

```bash

>  NX  Generating @nx/angular:library


 >  NX   Cannot find configuration for 'my-lib'

   Pass --verbose to see the stacktrace.
```

## Lerna config that breaks it

In your `lerna.json` file, change `packages` to `other/*`:

```json
{
  "$schema": "node_modules/lerna/schemas/lerna-schema.json",
  "packages": ["other/*"],
  "version": "0.0.0",
   ...
}
```

Now this works:

```bash
nx g @nx/angular:library --name=my-lib --buildable=true --publishable=true --importPath=@myorg/my-lib  --directory=libs/my-lib  --projectNameAndRootFormat=as-provided --dry-run
```
