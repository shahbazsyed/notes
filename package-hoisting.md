# Package Hoisting in node_modules

Hoisting in the context of npm and yarn is a dependency management technique that attempts to flatten the `node_modules` structure by moving compatible dependencies higher up in the directory tree (typically to the root `node_modules`). While it can offer benefits like reduced duplication, it also introduces the problem of "phantom dependencies," where your code might unintentionally rely on dependencies that are not explicitly declared in your `package.json`.  Modern package managers are evolving to address the downsides of traditional hoisting and provide more robust and predictable dependency management.

**What Hoisting Means (Generally):**

At its core, "hoisting" means **lifting or moving something higher up in a hierarchy or structure.**  Think of a physical hoist that lifts heavy objects.  In a technical context, it often refers to reorganizing elements within a system to be more accessible or efficient at a higher level.

**Hoisting in npm and Yarn (Package Manager Context):**

In the context of `npm` and `yarn` (especially versions before `npm v8` and `yarn v2+ Plug'n'Play/pnpm-like behaviour`), hoisting refers to a strategy used to manage dependencies in `node_modules`.  Let's visualize the typical dependency structure *without* hoisting:

Imagine you have a project with these dependencies:

* **Project A:**
    * `package-b` (version 1.0.0)
        * `package-c` (version 2.0.0)
    * `package-d` (version 3.0.0)
        * `package-c` (version 2.0.0)

Without hoisting, the `node_modules` structure might look something like this:

```
node_modules/
  package-b/
    node_modules/
      package-c/
  package-d/
    node_modules/
      package-c/
```

Notice that `package-c` is installed *twice*, once inside `package-b`'s `node_modules` and again inside `package-d`'s `node_modules`. This can lead to:

* **Duplication:**  Wasted disk space and potentially longer installation times.
* **"Dependency Hell" Potential:**  If different sub-dependencies require *conflicting* versions of a common dependency, it becomes more complex to manage.

**Hoisting to the Rescue (Traditional npm/yarn hoisting):**

Hoisting is a mechanism to **flatten** this nested dependency structure. Package managers try to "hoist" dependencies that are compatible to the **root `node_modules` directory**.  Using the same example, with hoisting, the `node_modules` structure might become:

```
node_modules/
  package-b/
  package-c/  <-- Hoisted to the root!
  package-d/
```

Now, `package-c` is only installed **once** in the root `node_modules`.  Both `package-b` and `package-d` can access it because Node.js's module resolution algorithm will look up the directory tree until it finds `node_modules/package-c`.

**Benefits of Hoisting:**

* **Reduced Duplication:**  Saves disk space and potentially download times.
* **Simplified Dependency Tree:** Makes the dependency structure conceptually flatter (though the actual file system might still have some nesting).
* **Potential Performance Improvements:**  Faster module resolution in some cases, as Node.js might find modules closer to the root.

**The Downside: "Phantom Dependencies" and the Example in Your Question**

Your example highlights the key **drawback** of hoisting: **"phantom dependencies."**

> "As a result, source code has access to dependencies that are not added as project dependencies."

Because packages are hoisted to the root, your project's code running in `node_modules/my-project/index.js` (for instance) can accidentally access dependencies that were hoisted from *sub-dependencies* of your project's direct dependencies.

**Example Breakdown:**

Let's say your `package.json` only lists `package-b` and `package-d` as direct dependencies.  You *haven't* explicitly declared `package-c`.  However, because `package-b` and `package-d` both depend on `package-c`, and `package-c` is hoisted, your code in `my-project/index.js` *might* be able to import and use `package-c`:

```javascript
// my-project/index.js
import packageC from 'package-c'; // This might work due to hoisting, even if 'package-c' isn't in my project's dependencies!

console.log("Using package-c:", packageC);
```

**Why is this a problem?**

* **Fragile Dependencies:** Your code becomes reliant on `package-c` *implicitly*. If:
    * You update `package-b` or `package-d` and they no longer depend on `package-c`.
    * The hoisting algorithm changes in a future version of npm/yarn and `package-c` is no longer hoisted to the root.
    * You switch to a different package manager that doesn't hoist in the same way (like pnpm or modern Yarn with Plug'n'Play).

...your code that *accidentally* relied on `package-c` will **break**.

* **Unclear Dependency Graph:** It becomes harder to understand your project's actual dependencies.  You might think you only depend on `package-b` and `package-d`, but you're secretly relying on `package-c`.

**Modern Package Managers and Hoisting (Evolving Landscape):**

* **npm v8+ and yarn v2+ (Plug'n'Play/pnpm-like):** Modern versions of npm and yarn have moved away from traditional hoisting or have significantly changed their hoisting strategies. They often use more sophisticated methods (like symbolic links or content-addressable storage) to manage dependencies in a more deterministic and less "phantom dependency" prone way.
* **pnpm:** pnpm is a package manager that was designed from the start to be non-hoisting (by default). It uses symbolic links to create a `node_modules` structure where only explicitly declared dependencies are directly accessible at the root level.  This eliminates phantom dependencies but can create a more deeply nested `node_modules` structure.
