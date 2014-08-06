#gulp-tsc-autoref

This plugin is used to automatically resolve your typescript references and add them to the top of the file so that you don't have to browse through your file system and write ugly relative paths that looks like '../../../../'.

## Instalation

```shell
npm install --save-dev gulp-tsc-autoref
```

## How does it work?

To use this plugin you must write your code in modules and your folder structure must follow the namespacing. For example:

```typescript
// Container.ts

import Injector = Vyper.Foundation.Injector;
import Injector = Vyper.Cache.CacheService;
```

```typescript
// Injector.ts

module Vyper.Foundation.Injector {
	// Your code here...
}
```

means that your folder structure must be the following:

```shell
|-- src
    |-- Vyper
        |-- Foundatin
			|-- Injector.ts
			|-- Container.ts
		|-- Cache
			|-- CacheService.ts
		|-- etc...
```

The process will add the following to the top of the `Container.ts` file:

```typescript
/// <reference path="Injector.ts" />
/// <reference path="../Cache/CacheService.ts" />
```

## Why?

Because I hate writing relative paths. I'm also lazy to specify my dependencies twice, once with `import` and once with `reference`.

## How to use it?

I suggest you compile your unreferenced typescript files into a build folder and run `tsc` on that. For example:

```javascript
var gulp        = require('gulp');
var typescript  = require('gulp-tsc');
var tscAutoref  = require('gulp-tsc-autoref');

gulp.task('default', function() {
    gulp.src(['src/**/*.ts'])
        .pipe(tscAutoref())
        .pipe(gulp.dest('dist/src'));

    gulp.src(['build/before.ts', 'dist/src/**/*.ts', 'build/after.ts'])
        .pipe(typescript({
            out: 'index.js',
            sourcemap: true
        }))
        .pipe(gulp.dest('./'))
});
```

## License

This open-sourced software is licensed under the [MIT license](http://opensource.org/licenses/MIT)

Copyright (c) 2014 Denis Pshenov

