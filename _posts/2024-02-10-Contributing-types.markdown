---
layout: post
title:  "Contribute type declarations for your favourite JavaScript library"
date:   2024-02-10 20:32:00 +0530
---

![Missing type declarations](/images/types/missing-type.png)

Frequently, when integrating a new library into your TypeScript project, you might encounter the above error. This error indicates that the library you just imported into your TypeScript project was authored in JavaScript and lacks associated type declarations. While you can attempt to install the type declaration from the `@types` namespace, there's a possibility that it hasn't been incorporated into [DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped) yet. You're faced with a couple of choices in this scenario: you can either disregard the warning, create a local d.ts file containing the module's type declaration, or contribute a type declaration to DefinitelyTyped.

Contributing type annotations for a JavaScript library not only simplifies its usage within a TypeScript project but also aids in enhancing your TypeScript skills. DefinitelyTypes streamlines the process of submitting types for a library in a few straightforward steps.

1. Check out the DefinitelyTyped [repo](https://github.com/DefinitelyTyped/DefinitelyTyped)

2. Generate the template for your project by executing `npx dts-gen --dt --name <my-package> --template module`. This action creates a directory `types/<my-package>` with the following structure:
  * `index.d.ts`: This file contains the typings for the package.
  * `<my-package>-tests.ts`: This contains sample code which tests the typings. This code does *not* run, but it is type-checked.
  * `tsconfig.json`: This allows you to run `tsc` within the package.
  * `.eslintrc.json`: (Rarely) Needed only to disable lint rules written for eslint.
  * `package.json`: Contains metadata for the package, including its name, version and dependencies.
  * `.npmignore`: Specifies which files are intended to be included in the package.

3. In the `package.json`, add any additional dependencies needed for the project, along with updating the `owners` list.

4. Define the type for your library within the `index.d.ts` file.

5. Incorporate your tests within the `<my-package>-tests.ts`. If your tests involve jsx, alter the extension from `.ts` to `.tsx` and reflect the change in `tsconfig.json`.

6. Validate tests for your library using `pnpm test <my-package>`.

7. Submit a pull request to [DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped/pulls); your types will be updated in the npm registry upon merging the PR.

For the most recent instructions, please refer to [this](https://github.com/DefinitelyTyped/DefinitelyTyped/blob/master/README.md) resource.
