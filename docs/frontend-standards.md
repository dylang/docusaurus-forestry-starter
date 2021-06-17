---
id: frontend-standards
title: Frontend Standards
sidebar_label: Frontend Standards

---
    
    
    
    # frontend-development-standards
    
    > Standards for development at Medallia
    
    
    This document is a work-in-progress. Please help it grow by adding to it via pull requests.
    
    
    ## ESLINT
    
    * Use [@m/eslint-preset-magic](https://github.medallia.com/Magic/magic/tree/main/config/eslint-plugin-magic).
    ```js
    /* istanbul ignore file */
    /* eslint-disable import/no-commonjs */
    /*
    To see the ESLint config:
    https://github.medallia.com/Magic/magic/blob/main/config/eslint-plugin-magic/src/eslintrc.js
    */
    
    const DISABLED = 0;
    
    module.exports = {
      extends: ['plugin:@m/magic/recommended'],
      rules: { 
        // Rules can be disabled if they do not apply, but always explain why.
        // This project is not going to be internationalized. 
        'react/jsx-no-literals': DISABLED
      }
    };
    ```
    * Rules that do not auto-fix can be disabled, but you must include a comment explaining why it was disabled. Ideally, also include a ticket for enabling disabled rules.
    * All files must pass lint to merge, warnings are not permitted.
    * Configure your editor to run `eslint --fix` on save.
    * TODO: Update @m/eslint-preset-magic readme, out of date info regarding Prettier and tslint.
    * TODO: Update .eslintrc.js in magic-demo.
    
    ## PRETTIER
    
    * `@m/eslint-preset-magic` automatically run Prettier for code formatting.
    * Magic Prettier config defaults to four space tabbing, but this can be configured:
    ```js
    /* istanbul ignore file */
    /* eslint-disable import/no-commonjs */
    
    // https://prettier.io/docs/en/options.html
    module.exports = {
        // https://github.medallia.com/Magic/magic/blob/main/config/eslint-plugin-magic/src/prettier.config.js
        ...require('@m/eslint-plugin-magic/src/prettier.config')
        // Example change, use two-space tabs instead of 4.
        // tabWidth: 2
    };
    ```
    * TODO: Update prettier config link in Magic demo.
    
    ## TYPESCRIPT
    
    * All new projects must be written in TypeScript.
    * All current JS that is actively being maintained should be transitioning to TypeScript.
    * tsconfig: Always extend [@m/magic-typescript](https://github.medallia.com/Magic/magic/tree/main/config/magic-typescript).
    ```json
    {
        "extends": "@m/magic-typescript",
        "compilerOptions": {
            "rootDir": "."
        }
    }
    ```
    * Is the package something that will be used in a browser? Extend `@m/magic-typescript/web`.
    * Is the package transitioning from JS to TypeScript? Extend `@m/magic-typescript/allowJs`
    
    ## FILENAMES
    
    * Always use `kebab-case-filenames.ts`.
    * `index.ts` should only be used to export other files. Never hide business logic in files called `index.ts`.
    
    ## TESTING
    
    * Use Jest for all tests.
    * Unit tests must live next to the code they test (not in a "test" directory).
    * Unit tests must have the filename <file they are testing>.test.ts. Never spec.js.
    * Browser tests must use Argus and be written in TypeScript.
    * Keep test output clean. Tests should never output anything to the console. Mock the console in your test. As a bonus, you can expect() to see if the console messages are what you expect.
    * Keep auto-mocking to a minimum. Automocking is slow and not perfect.
    ```ts
    jest.mock('module-name'); // no implementation, jest has to run the code to figure it out.
    jest.mock('module-name', () => ({ foo: jest.fn() })); // foo is a mocked export.
    ```
    * Use `react-testing-library`. Do not use Enzyme. 
    
    ## PACKAGE.JSON
    
    * The name of every package must start with `@m/` so that we never overwrite or can be overwritten by a public package.
    * Use `"license": "UNLICENSED"`, don't accidentally open source your work.
    * In monorepos, always use the version `0.0.0` for all packages. This avoids merge conflict hell when releasing new versions, and makes it clear to other developers that the package is from the same repo. 
    
    ## PACKAGE MANAGER
    
    * Always use `yarn@v1`. 
    * Always check in the `yarn.lock`. This is the contract that ensures what you installed locally is exactly the same versions as Jenkins will install and all other developers.
    * Use [Renovate](https://github.medallia.com/medallia/renovate) to keep your dependencies and lockfile up-to-date. Configure it to run either every weekend or at nights.
    
    ## GITHUB 
    
    * Enable these settings for your repos
      * `Merge button`:
        * Disable `Allow merge commits`
        * Enable `Allow squash merging` <- Pull requests are squashed automatically. Never ask developers to do this manually.
        * Disable `Allow rebase merging`
      * `Branch Protection`:
        * `Require pull request reviews before merging`
        * `Require status checks to pass before merging`
    * Never rebase and force-push pull requests. This destroys your personal history, loses code review comments, and forces reviewers to have to "start over" instead of just looking at new changes.
    * When making pull requests, fork the repo and make the changes in a branch in your own repo. This way we can keep the origin clean and only include branches that need to live forever, like release branches.
    * Always ask for a code review on Slack. Never assume somebody will just start reviewing your code, even if you assign it to them on Github.