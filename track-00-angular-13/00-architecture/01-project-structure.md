# Level 1 — The Big Separation
Your project has two worlds:

- 1️⃣ Tooling / Configuration World

(Used by Angular CLI, TypeScript, Build system)

- 2️⃣ Application World

(What actually runs in the browser)

If you don’t separate these mentally, you’ll confuse everything.

# Level 2 — Root Folder (Tooling World)

<img width="340" height="434" alt="image" src="https://github.com/user-attachments/assets/50921201-e06e-40de-9dd1-25da9d102f59" />

Outside src/ you see:

```sql
.angular
.vscode
node_modules
angular.json
package.json
tsconfig.json
karma.conf.js
```

These are NOT your business logic.

Let’s understand them clearly.

## node_modules

### What is it?

The physical folder where all installed packages live.

- Angular framework code lives here.
- RxJS lives here.
- TypeScript lives here.

### When is it used?

Build time AND runtime (indirectly).
- At build time:

Bundler reads from node_modules.
- At runtime:

Compiled Angular code originates from these packages.

### What problem does it solve?

1. Prevents you from manually copying framework code.
2. Centralizes third-party dependencies.

### What happens if it is missing?

1. Project will not compile.
2. Run npm install to regenerate.

# package.json

## What is it?

A dependency and project configuration file for Node.js.

It defines which libraries your project depends on and what scripts can be executed.

## When is it used?

Build time.

It is read when:

- You run npm install
- You run ng serve
- You run ng build

## What problem does it solve?

Without it:

1. There is no way to know which Angular version you use.
2. There is no way to install required dependencies.
3. Teams would install different versions manually.

It ensures:
1. Version consistency.
2. Reproducible builds.

## What happens if it is missing or misconfigured?

If missing:
1. npm cannot install dependencies.
2. Angular CLI cannot run.

If Angular version is wrong:
1. Incompatibility errors.
2. Unexpected runtime behavior.


# angular.json

This is extremely important.

## What is it?

The Angular CLI configuration file.
It tells Angular how to build your app.

It defines:

- Entry file (main.ts)
- Styles
- Assets
- Output path
- Production optimizations

## When is it used?

Build time only.

## Angular CLI reads it when:
1. ng serve
2. ng build
3. ng test

## What problem does it solve?
Without it:

1. CLI wouldn’t know where your app starts.
2. It wouldn’t know which CSS to bundle.
3. It wouldn’t know what to copy to dist folder.
It standardizes build behavior.

## What happens if misconfigured?

If "main": "src/main.ts" is wrong:

- Angular fails to bootstrap.

If styles path wrong:

- Global styles don’t load.

If assets misconfigured:

- Images won’t appear.

# tsconfig.json

## What is it?

TypeScript compiler configuration file.

It defines:

1. ECMAScript target
2. Strict mode
3. Path aliases
4. Module resolution

## When is it used?
Build time only.

TypeScript compiler reads it before converting .ts → .js.

## What problem does it solve?

Ensures:

1. Consistent TypeScript behavior.

2. Strong typing.

3. Clean imports via path mapping.

## What happens if misconfigured?

Wrong target:

- Browser incompatibility.

Strict mode off:

- Bugs slip through.

Path mapping wrong:

- Imports break.






