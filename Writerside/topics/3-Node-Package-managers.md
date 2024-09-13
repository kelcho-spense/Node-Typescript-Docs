# 3. Node Package managers

Node.js uses various package managers to handle external libraries and dependencies, which are essential for building modern applications. These package managers simplify the process of installing, updating, and managing the libraries your project requires. In this section, we'll cover what a package is, and introduce the most commonly used package managers: **NPM**, **PnPm**, and **Yarn**.

#### What is a Package?

In the Node.js ecosystem, a **package** refers to a reusable piece of code or library that performs a specific task or provides specific functionality. Packages are distributed as modules, which can include utilities, frameworks, middleware, or even entire applications.

A typical package may consist of:
- JavaScript/TypeScript code
- A `package.json` file containing metadata (name, version, dependencies, etc.)
- Additional resources like README files, license agreements, and configuration files

Packages can be installed locally (only available to the project) or globally (available system-wide). They are often hosted in package registries like the official **npm registry**.

Example of a `package.json` file:
```json
{
  "name": "my-node-app",
  "version": "1.0.0",
  "description": "A simple Node.js app",
  "main": "index.js",
  "dependencies": {
    "express": "^4.17.1"
  },
  "devDependencies": {
    "typescript": "^4.4.3"
  },
  "scripts": {
    "start": "node index.js",
    "build": "tsc"
  }
}
```
In this example, the `express` package is listed as a runtime dependency, while `typescript` is a development dependency.

---

#### NPM (Node Package Manager)

**NPM (Node Package Manager)** is the default package manager for Node.js, and it comes pre-installed when you install Node.js. NPM allows developers to easily share and reuse code and manage dependencies.

##### Key Features:
- **Largest Package Registry**: NPM has the largest repository of open-source Node.js libraries available for installation.
- **Dependency Management**: It manages both direct and transitive dependencies through a `package.json` file.
- **Scripts**: NPM allows you to define custom scripts in `package.json` to automate tasks like testing, building, or deploying.
- **Versioning**: NPM makes it easy to update or lock dependencies to specific versions, ensuring that your project uses stable versions of packages.

##### Common NPM Commands:

- Install a package locally:
  ```bash
  npm install <package-name>
  ```

- Install a package globally:
  ```bash
  npm install -g <package-name>
  ```

- Add a package as a development dependency:
  ```bash
  npm install <package-name> --save-dev
  ```

- Install all dependencies listed in `package.json`:
  ```bash
  npm install
  ```

- Remove a package:
  ```bash
  npm uninstall <package-name>
  ```

- Running a custom script defined in `package.json`:
  ```bash
  npm run <script-name>
  ```

##### Example:
```bash
npm install express
```

This command installs `express`, a web framework, and updates `package.json` with the new dependency.

---

#### c. PnPm

**PnPm** is an alternative package manager for Node.js that aims to optimize disk space and speed. It addresses some of the inefficiencies of NPM and Yarn by storing package dependencies in a shared, central directory, and linking them into each project. This can significantly reduce disk usage, especially when multiple projects have similar dependencies.

##### Key Features:
- **Efficient Disk Usage**: PnPm uses a content-addressable storage to store every package only once, even if multiple projects depend on different versions of the package.
- **Performance**: It is often faster than both NPM and Yarn, especially for monorepo projects.
- **Strict Dependency Management**: PnPm ensures that the dependency tree is more predictable, catching issues like missing or incompatible peer dependencies early on.

##### Common PnPm Commands:

- Install a package locally:
  ```bash
  pnpm install <package-name>
  ```

- Install a package globally:
  ```bash
  pnpm add -g <package-name>
  ```

- Add a development dependency:
  ```bash
  pnpm add <package-name> --save-dev
  ```

- Install all dependencies listed in `package.json`:
  ```bash
  pnpm install
  ```

##### Example:
```bash
pnpm install express
```

This command installs `express`, just like NPM, but PnPm will link the dependencies in an optimized way, saving disk space.

##### Advantages:
- Faster installation times in larger projects.
- Prevents projects from installing unnecessary duplicate dependencies.
- Strictness in dependency resolution ensures fewer conflicts.

---

#### d. Yarn

**Yarn** is another package manager that was originally created by Facebook to address some performance and security concerns they found in NPM. Yarn improves on speed and consistency, offering a lockfile system and better offline support.

##### Key Features:
- **Fast Installation**: Yarn caches every package it downloads, so it never needs to download the same package again.
- **Lockfile for Consistency**: Yarn generates a `yarn.lock` file to ensure that all developers working on a project install exactly the same package versions.
- **Parallel Installations**: Yarn installs packages in parallel, which can significantly reduce installation times.
- **Offline Mode**: Once a package is installed, it can be reinstalled without an internet connection, thanks to Yarn’s caching system.

##### Common Yarn Commands:

- Install a package locally:
  ```bash
  yarn add <package-name>
  ```

- Install a package globally:
  ```bash
  yarn global add <package-name>
  ```

- Add a development dependency:
  ```bash
  yarn add <package-name> --dev
  ```

- Install all dependencies listed in `package.json`:
  ```bash
  yarn install
  ```

##### Example:
```bash
yarn add express
```

This command adds `express` as a dependency to your project using Yarn.

##### Advantages:
- **Faster** than NPM due to parallel package installation.
- **Offline Mode** allows reinstallation of previously installed packages without an internet connection.
- **Deterministic**: The `yarn.lock` file ensures that every installation will result in the same file structure.

---

### Summary

| Feature         | NPM                           | PnPm                          | Yarn                          |
|-----------------|-------------------------------|-------------------------------|-------------------------------|
| **Speed**       | Standard                      | Very fast                     | Fast                          |
| **Disk Usage**  | Can be heavy on disk space     | Optimized with central storage| Efficient with caching        |
| **Offline Mode**| Limited                        | No, but faster for cached files | Yes                           |
| **Dependency Tree** | Standard                   | Strict                         | Flexible but with lockfiles   |
| **Parallel Installs**| No                        | Yes                            | Yes                           |

In conclusion, NPM, PnPm, and Yarn all have their strengths and weaknesses, and your choice of package manager depends on your specific needs. **NPM** is the default and easiest to use, **PnPm** optimizes disk space and speed for large projects, and **Yarn** offers performance improvements and more deterministic installs.