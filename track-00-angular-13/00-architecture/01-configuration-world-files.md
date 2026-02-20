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
node_modules
.browserlistrc
.editorconfig
.gitignore
angular.json
karma.conf.js
package-lock.json
package.json
README.md
tsconfig.app.json
tsconfig.json
tsconfig.spec.json
```

These are NOT your business logic.

Let’s understand them clearly.

# node_modules

## What is it?

The physical folder where all installed packages live.

- Angular framework code lives here.
- RxJS lives here.
- TypeScript lives here.

## When is it used?

Build time AND runtime (indirectly).
- At build time:

Bundler reads from node_modules.
- At runtime:

Compiled Angular code originates from these packages.

## What problem does it solve?

1. Prevents you from manually copying framework code.
2. Centralizes third-party dependencies.

## What happens if it is missing?

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

# The Remaining Files

## 1️⃣ Build Acceleration & Internal CLI State

### .angular/
This folder exists purely for Angular CLI internal caching.

Think of it as:
- “Performance memory for Angular.”

When You run:
````sql
ng serve
````
Angular caches parts of compilation here so next build is faster.

If deleted:

- Nothing breaks.
- First build will be slower.
- It regenerates automatically.

You don’t touch this folder.

This is internal machinery.

## 2️⃣Dependency Stability & Reproducibility

### package-lock.json

This is not just “another JSON file.”

This is the file that locks every single dependency to exact versions — including sub-dependencies.

Example:

Even if you say:
- "@angular/core": "^13.0.0"

package-lock.json pins it to something like:
- "@angular/core": "13.0.3"

Why this matters?

Without it:
1. Developer A installs slightly newer sub-dependency.
2. Developer B installs slightly older one.
3. One of them gets a random build error.
4. Debugging becomes painful.


## 3️⃣ Code Style & Team Discipline

### .editorconfig

This file defines formatting rules:
- Indentation
- Line endings
- Character encoding

In a team:
Without this:
- One developer uses tabs.
- Another uses spaces.
- Git diff becomes noisy.
- Code reviews become frustrating.

It has nothing to do with Angular runtime.

It has everything to do with professionalism.

## 4️⃣ Version Control Hygiene

### .gitignore

This tells Git:
- Do not track these files.

Usually ignores:

- node_modules/
- dist/
- logs
- temporary files

Without it:

- You accidentally push node_modules (thousands of files).
- Your repo becomes huge.
- CI slows down.

This file protects repository cleanliness.

## 5️⃣ Browser Compatibility

### .browserslistrc

This tells Angular:

“Which browsers must this app support?”

If you support older browsers:

1. Angular transpiles differently.
2. Polyfills are added.

If you only support modern Chrome:

1. Smaller bundle.
2. Less polyfills.

This file affects build output.

It influences how modern your JavaScript can be.

## 6️⃣ Testing Infrastructure

## karma.conf.js
## tsconfig.spec.json

These exist for unit testing.

Angular by default uses:
1. Karma
2. Jasmine

karma.conf.js:
- Configures test runner behavior.

tsconfig.spec.json:
- TypeScript config for test files.

These files are not required for runtime.
They are required for testing reliability.

## 7️⃣ TypeScript Separation

### tsconfig.app.json
### tsconfig.json

tsconfig.json:
- Base configuration.

tsconfig.app.json:
- Used for compiling the actual app.

Angular separates app compilation from test compilation.

This allows flexibility and clarity.

## 8️⃣ Documentation

### README.md

Human-facing documentation.

Important for:
1. Onboarding
2. Deployment instructions
3. Context

A good README reduces onboarding time significantly.

---

Now Step Back

Look at the project root again.

Every file outside src/ belongs to one of these categories:

1. Build & CLI control

2. Dependency management

3. TypeScript configuration

4. Testing configuration

5. Team discipline

6. Version control hygiene

7. Documentation

Nothing is random.

Angular CLI generates a structured environment, not just an app.


