# AngularJS Directives

## Chapter 5: Keeping it clean with Scope

The default scope value for directives is `false`. With `false` the directive receives the same scope as its parent.

There are 3 different ways to specify isolate scope in a directive:

    scope: {
        title: "@title", // read only scope
        subtitle: "=subtitle", // two way
        method: "&method" // method bindind (complicated...)
    }

