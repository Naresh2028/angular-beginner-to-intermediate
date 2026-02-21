sss






## Why Not Just Use Folders?

- Folders are for humans
- NgModule is for Angular

Angular does not understand folders.

It understands meta data.

So this:
- Login/Login.component.ts

means nohting to Angular.

But this:
````Sql
@NgModule({
  declarations: [LoginComponent]
})
export class LoginModule {}
````
Now Angular understands:











