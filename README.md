## Firebase Function Boilerplate Template (Typescript)

This repository houses basic typescript firebase configuration with a hello-world example function. The aim here is to setup a seamless and reusable deployment across multiple environment and projects by leveraging Github Environments as well as a combination of some dope dev-tools.

## Dev Tools

### [Nodemon](https://www.npmjs.com/package//nodemon)

nodemon is a tool that helps develop Node.js based applications by automatically restarting the node application when file changes in the directory are detected.

Checkout the `dev` script in the `package.json` to see the setup.

### [Prettier](https://prettier.io/docs/en/install.html)

Prettier is an opinionated code formatter. We foramt files with prettier at push by using a husky hook. This ensures that our files are neat and tidy and that nothing gets overwritten by local formatters.

Note - Installing and using the VSCode prettier extension locally + format on save is a vibe.

### [CommitLint](https://commitlint.js.org/#/)

Helps your team adhere to a commit convention. By supporting npm-installed configurations it makes sharing of commit conventions easy.

Here we're enforcing [conventional commit](https://www.conventionalcommits.org/en/v1.0.0/) standards. These are enforced on each commit by using a husky hook.

### [Husky](https://typicode.github.io/husky/)

Modern native git hooks made easy. Checkout the `.husky` folder to see what we've got going there.

### [ESLint](https://eslint.org/)

ESLint is a static code analysis tool for identifying problematic patterns found in JavaScript code

## Git Hooks

### pre-commit

Format staged files using `prettier` via `lint-staged`.

### commit-msg

Enforce conventional commit messages using `commitLint`.

### pre-push

Run `eslint` to ensure that there are no errors. This one can be annoying and this can be skipped by runing `git push --force` if you're trying to just push a branch that's still under construction.

### post-merge

Runs `npm install` to sync any dependency changes or updates that might have been pulled.

## Deployment Triggers & Github Environments

The basic setup here is to trigger deployment on pushes to the `main` and `develop` branches.

```
on:
  push:
      branches: [main, develop]
```

We have environments setup in Github for each of these branches with the exact same names. i.e - `main` and `develop`. This makes tying the correct environment to their respective branches super easy

```
environment: ${{ github.ref_name }}
```

Now variables that differ based on the environment will correctly be pulled and deployed.

For instance when we want to deploy to a different project based on the environment (testing/production) we can set the `PROJECT_ID` variable in each environment and then let the deployment script select the correct one

```
run: firebase deploy
--token "${{ secrets.FIREBASE_TOKEN }}"
--only functions
--project ${{ vars.PROJECT_ID  }}
```

Note - The `FIREBASE_TOKEN` secret is set a repository level so that remains consistent regardless of the environment

## Getting Started

- Fork this repository :fork_and_knife:
- Tune the configs to your liking
- Run `npm run dev` to get in the kitchen and start cooking!
