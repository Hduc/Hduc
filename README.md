
![](https://miro.medium.com/v2/resize:fit:700/1*rI0VMLMIarstY7nICV5kgQ.png)

Introduction
============

Why did I decide to write one more article about publishing npm packages? When I needed to create such a package by myself, I found that most of the instructions on the internet were either outdated or very short and useless in production.

I will try to describe all the steps as clearly as I can, so you can create your own project from the scratch. Let’s get started!

I propose to create a simple React application, which will consist of two buttons that increase or decrease the value of the counter.

This is what our application will look like:

Demo: [https://gapon2401.github.io/my-react-typescript-package/](https://gapon2401.github.io/my-react-typescript-package/)  
Repo: [https://github.com/gapon2401/my-react-typescript-package](https://github.com/gapon2401/my-react-typescript-package)

We have 14 steps that deal with everything:

1.  [Preparing the project](#a15d)
2.  [Create your react application](#48c1)
3.  [Configure git](#bf2d)
4.  [Configure ESLint](#0e1f)
5.  [Configure Prettier](#4760)
6.  [Adding tests with Jest](#7b1c)
7.  [Configure package.json and prepare for publishing](#12f8)
8.  [Commit and push your code](#8cf2)
9.  [Publishing to NPM](#b9e9)
10.  [Creating the example folder](#38de)
11.  [Setup automated builds](#b8a5)
12.  [Demo page](#a5ea)
13.  [README.md file and shields.io](#8891)
14.  [Making release](#2a4f)

**Step 1. Preparing the Project**
=================================

*   In the command line of your project, execute:

npm init -y

It will create `package.json` file with default values, and we will change it a bit later.

*   Create folder `src`

Here we will keep all project files.

*   Add React and TypeScript to the project (I’m going to use `yarn` , if you don’t mind)

yarn add -D react react-dom typescript @types/react

Now we have `node_modules` folder and a `yarn.lock`— not bad!

*   In order to compile TypeScript, create a `tsconfig.json`file in the project root with the following content:

tsconfig.json

You can take a look at all possible properties here: [https://www.typescriptlang.org/tsconfig](https://www.typescriptlang.org/tsconfig)

From the main:

\- `rootDir` — the root folder within your source files

\- `include`— specifies the files to be included in the TypeScript project

\- `exclude` — specifies the files to be excluded in the TypeScript project. We don’t want to include `dist` folder and all `node_modules`

\- `outDir` — this is the location for the compiled output

*   Create empty `README.md` and `LICENSE`files

It’s up to you what license to use. I will use MIT. You can check the contents of that file.

Take a look at the structure we should have:

![my-react-typescript-package, node_modules, src, license, package.json, readme.md, tsconfig.json, yarn.lock](https://miro.medium.com/v2/resize:fit:209/1*7jzC3s7hcElCZmA5zIICvA.jpeg)

Project structure after step 1

Step 2. Create Your React Application
=====================================

Inside `src` create the folder `components`. There you can keep all the components of your project. We will create here within this article only one file `App.tsx` with the following content:

src/components/App.tsx

This is our React application.

Now, create a file `index.ts` inside the `src` folder. Export the module.

src/index.ts

It’s time to compile the project!

Let’s change our `package.json` and replace the `scripts` section:

package.json

Run the following command in the terminal:

yarn build

Folder `dist` should be appeared with all compiled code.

Congratulations! We have created the application, which is compiled. A few more steps and we’re done! (Not.)

Compare your project structure after step 2:

![my-react-typescript-package, dist, esm, components, app.d.ts, app.js, app.js.map, index.d.ts, index.js, index.js.map, my-react-typescript-package, node_modules, src, license, package.json, readme.md, tsconfig.json, yarn.lock](https://miro.medium.com/v2/resize:fit:251/1*1BS6RtOlS5cp0Rw9-qIYpw.jpeg)

Project structure after step 2

**Step 3. Configure git**
=========================

We quickly created the project; now it’s time for the git.

Initialize `git` in the root of the project with this command:

git init

Create the `.gitignore`file in the project root with the following content:

node\_modules  
.idea  
dist  
yarn-error.log

I added `.idea` , because I’m developing with the help of Jetbrains IDE.

In `.gitignore` we list those files and the directories we would like not to include in the repository.

Create a git repo on [GitHub](https://github.com/). Later, we will create an initial commit and connect remote git with our local git.

**Step 4. Configure ESLint**
============================

ESLint statically analyzes your code to quickly find problems. We need it only in development.

Add eslint and all necessary components with this code:

yarn add -D eslint eslint-plugin-react eslint-plugin-react-hooks @typescript-eslint/eslint-plugin @typescript-eslint/parser

You can configure it as you want; there is a big user guide: [https://eslint.org/docs/user-guide/configuring/](https://eslint.org/docs/user-guide/configuring/)

I suggest you create an `.eslintrc` file at the root of the project:

.eslintrc

Add `.eslintignore`:

node\_modules  
dist

We will ignore checking `dist` folder with compiled files and `node_modules` .

Change `package.json` , add to `scripts` section:

"lint": "eslint \\"{\*\*/\*,\*}.{js,ts,jsx,tsx}\\""

Here is my `package.json`:

package.json

Now, run the following:

yarn lint

You shouldn’t have any mistakes.

Compare your project structure after step 4:

![my-react-typescript-package, dist, node_modules, src, components, app.tsx, index.ts, .eslintignore, .eslintrc, .gitignore, license, package.json, readme.md, tsconfig.json, yarn.lock](https://miro.medium.com/v2/resize:fit:228/1*pKiGZXK4YRmOiK9UHYygNQ.jpeg)

Project structure after step 4

**Step 5. Configure Prettier**
==============================

Prettier is a code formatter. It is convenient to use it when you work in a team so that everyone’s code meets the same standard.

In the terminal, run the following command:

yarn add -D eslint-config-prettier eslint-plugin-prettier prettier

In the project root, create `.prettierrc.json`:

`.prettierrc.json`

Read more about all options: [https://prettier.io/docs/en/options.html](https://prettier.io/docs/en/options.html).

Add a prettier plugin to `.eslintrc`:

.eslintrc

We have finished the setup of prettier; now try to run `yarn lint` in order to check formatting errors. If you don’t have any, you didn’t pass the human test 😆.

![](https://miro.medium.com/v2/resize:fit:700/1*MnyzqNsfxzsY8JSRLa8EpQ.jpeg)

Eslint prettier errors

It’s time to fix them! Add the following to `scripts`in `package.json`:

"prettier": "prettier --write \\"{src,tests,example/src}/\*\*/\*.{js,ts,jsx,tsx}\\""

OK, run `yarn prettier` and you will get something similar to:

![> prettier — write…](https://miro.medium.com/v2/resize:fit:472/1*hOmMsj1ZTUuLGZJoMB1PrQ.jpeg)

yarn prettier

Now, after `yarn lint` you should not have any errors. Check the files `src/components/App.tsx` and `src/index.ts` , they have changed.

My `scripts` section in `package.json`:

"scripts": {  
  "build": "tsc",  
  "lint": "eslint \\"{\*\*/\*,\*}.{js,ts,jsx,tsx}\\"",  
  "prettier": "prettier --write \\"{src,tests,example/src}/\*\*/\*.{js,ts,jsx,tsx}\\""  
}

Compare your project structure after step 5:

![my-react-typescript-package, dist, node_modules, src, components, app.tsx, index.ts, .eslintignore, .eslintrc, .gitignore, prettierrc.json, license, package.json, readme.md, tsconfig.json, yarn.lock](https://miro.medium.com/v2/resize:fit:242/1*MhQmIV5eD213F3mmQu535w.jpeg)

Project structure after step 5

**Step 6. Adding Tests with Jest**
==================================

To create tests, I’m using the [Jest](https://jestjs.io/) library. It’s a pretty useful and powerful testing framework. How difficult the tests will be, of course, is up to you.

Add `jest` to your project:

yarn add -D jest jest-canvas-mock jest-environment-jsdom ts-jest @types/jest @testing-library/react

Create the project root file `jestconfig.json`:

`jestconfig.json`

Create folder `tests` .

Time to write our first test. We will write a very simple test, which will check that our render passed successfully.

In the folder `tests`, create file `common.test.tsx`:

tests/`common.test.tsx`

Change `scripts` section in `package.json`:

"test": "jest --config jestconfig.json"

`package.json`

Run `yarn test`. You should pass the test:

![Test passed](https://miro.medium.com/v2/resize:fit:328/1*OK2zYpAl1iSyV05Q9qvnyQ.jpeg)

Test passed

Compare your project structure after step 6:

![my-react-typescript-package, dist, node_modules, src, components, app.tsx, index.ts, .eslintignore, .eslintrc, .gitignore, prettierrc.json, jestconfig.json, license, package.json, readme.md, tsconfig.json, yarn.lock](https://miro.medium.com/v2/resize:fit:224/1*qkmCvH3e69v9GL0FwGMs8Q.jpeg)

Project structure after step 6

Step 7. Configure package.json and Prepare for Publishing
=========================================================

I suggest splitting our `build`. We will compile not only the ECMAScript module but also the CommonJs module to make our package as accessible as possible.

In `package.json`, replace:

"build": "tsc",

With the following code:

"build": "yarn build:esm && yarn build:cjs",  
"build:esm": "tsc",  
"build:cjs": "tsc --module commonjs --outDir dist/cjs",

We added `build:esm` and `build:cjs` and combined them in one command.

Run `yarn build` and you will see, that our `dist/` now has also a `cjs` folder.

Go further.

In `package.json` we can use magic scripts that are automatically called when the package is published. They will help us to check our package for all kinds of errors and not accidentally upload an update that will crash hundreds of thousands of projects in which our package will be used!

*   `prepare` — runs before the package is packed and published. Runs on local npm install without any arguments,
*   `prepublishOnly` — runs before the package is prepared and packed, only on npm publish. Here will be our tests.

Add the following to the `scripts` section:

"prepare": "npm run build",  
"prepublishOnly": "npm test && npm run prettier && npm run lint"

Change the `version` , `description`.

Tell npm where it can import the main file of our project from, and also where all the types are located:

Replace:

"main": "index.js",

With:

"main": "./dist/cjs/index.js",  
"module": "./dist/esm/index.js",  
"types": "./dist/esm/index.d.ts",

Add information about the remote git repository (specify your git URL from step 3):

"repository": {  
  "type": "git",  
  "url": "git+https://github.com/gapon2401/my-react-typescript-package.git"  
},

Specify that the project that will use our package must have a react version of at least >=16:

"peerDependencies": {  
  "react": ">=16"  
},

To ensure that your package does not have any redundant files, use only allowed files and folders that will be added to the package:

"files": \[  
  "dist",  
  "LICENSE",  
  "README.md"  
\],

Add keywords so everyone can find your project:

"keywords": \[  
  "react",  
  "typescript",  
  "awesome-project"  
\],

Specify your license:

"license": "MIT",

Don’t forget to change the `author`:

"author": "Igor Gaponov (gapon2401)",

I think that’s enough.

Add a description to your `README.md` file.

For now, I have only one row there. This is the h1:

\# my-react-typescript-package

Take a look at the final version of `package.json`:

package.json

Step 8. Commit and Push Your Code
=================================

First of all, let’s connect remote and local repositories and push our project.

Run the following code:

git add .  
git commit -m "Initial commit"  
git remote add origin <Repository Url>  
git push -u origin master

Don’t forget to change `<Repository Url>` on yours. It can be `https` URL, or `ssh` . This URL you can get in your repository.

Take a look at examples of how it could be.

*   HTTPS repository URL (you will need to authorize)

git remote add origin https://github.com/gapon2401/my-react-typescript-package.git

*   SSH repository URL (you need to configure [connecting with ssh](https://docs.github.com/en/authentication/connecting-to-github-with-ssh))

git remote add origin git@github.com:gapon2401/my-react-typescript-package.git

I used the second solution.

**Step 9. Publishing to NPM**
=============================

*   Choose the package name

We have to check that the name of our package is free to use. Go to the website [https://www.npmjs.com/](https://www.npmjs.com/) and use the search tool.

You can find and change the default name of the project in `package.json`:

![npm package name](https://miro.medium.com/v2/resize:fit:388/1*ArUS5eFO7TiTv6pS7Figvg.jpeg)

package name

In my case it is `my-react-typescript-package`. There is no such package yet.

I prefer to check this way: insert the name directly into the link, as shown below:

https://www.npmjs.com/package/<Your package name>

Instead of `<Your package name>`, use the name of your project.

I get the following URL:

[https://www.npmjs.com/package/my-react-typescript-package](https://www.npmjs.com/package/my-react-typescript-package)

If the name is free, you will see a 404 page:

![Package name is free](https://miro.medium.com/v2/resize:fit:536/1*E02Cvaa37971TbP_i-rzLQ.jpeg)

The package name is free

*   Register/login to NPM

In order to publish the project, you need to authorize in [https://www.npmjs.com/](https://www.npmjs.com/).

If you don’t have an account yet, go and register [https://www.npmjs.com/signup](https://www.npmjs.com/signup) or use the command line:

npm adduser

You will be asked to enter `username`, `password`and `email`.

If you already have an account, run:

npm login

And complete the authorization.

*   Publishing

Before publishing, I recommend you check that you will have only the necessary files in the package, there are no errors, and everything is formatted correctly. For this, you can run the following command:

npm publish --dry-run

The package will not be published, don’t worry.

You will see that the “magic script” `prepublishOnly` will be called and tests and code formatting will run.

Then you will get the list of all files, which will be included at the project:

![The list of all package files](https://miro.medium.com/v2/resize:fit:480/1*W2p_ex9oVW8CMgnMoEnnyQ.jpeg)

The list of all package files

In `Tarball Contents`, we have the files and folders that were specified in `package.json`:

"files": \[  
   "dist",  
   "LICENSE",  
   "README.md"  
 \],

Everything is fine; let’s publish the package!

Run the following command:

npm publish

I always worry when I run this command.

I am attaching the full result of the command call:

![The result of npm publish](https://miro.medium.com/v2/resize:fit:488/1*GWkaNAHWok9uWy9NrNpphw.jpeg)

The result of npm publish

Now, I can congratulate you! The package is published; it is public, and it can be used by anyone! It wasn’t so hard.

Let’s take a look at the package on the NPM.

Open the link we opened before.

https://www.npmjs.com/package/<Your package name>

![NPM page of the package](https://miro.medium.com/v2/resize:fit:700/1*h-VhOjLVxi9uFXbbtMzRpQ.jpeg)

NPM page of the package

Looks great!

**Step 10. Creating the Example Folder**
========================================

Let’s keep making our package better and better. In this step, we will create an example folder in which we will show how we can work with our package. In this folder, you can quickly start the server and play around with the code.  
To quickly launch the application, we will use [https://parceljs.org/](https://parceljs.org/).

Create a folder `example` .

Inside that folder, create an empty `src` folder and files:

*   `.gitignore`:

node\_modules  
yarn.lock  
.parcel-cache  
dist  
yarn-error.log

*   `package.json`:

example/package.json

*   `README.md`:

example/README.md

Now, pay attention! In the command line from the root of your project, move into the `example` folder:

cd example

Then run:

yarn add -D parcel my-react-typescript-package react react-dom @types/react-dom @types/react

Inside the `src` folder, create the file `index.html`:

example/src/index.html

Now, we need to import our package and create an example of how to use it.

Inside `src` folder, create `index.tsx` file:

example/src/index.tsx

Time to run the server! Use the command:

yarn start

You will have the following message after successfully starting the server:

![Parcel server started](https://miro.medium.com/v2/resize:fit:292/1*eJ5Y9RyNzmzWDXW6NKXTNw.jpeg)

Parcel server started

Open the link `[http://localhost:1234](http://localhost:1234)` to watch the example:

![Package example of usage](https://miro.medium.com/v2/resize:fit:465/1*bQN85v5WpnWvm_eW--1ZaQ.jpeg)

Package example of usage

Compare the example folder structure after all installations and servers are running. It should look like this:

![example, .parcel-cache, dist, node_modules library root, src, index.html, index.tsx, .gitignore, package.json, readme.md, yarn.lock](https://miro.medium.com/v2/resize:fit:260/1*EcBPHw6yjJI-r13ZTtIPCQ.jpeg)

example folder structure

**Step 11. Setup Automated Builds**
===================================

Let’s get into automated builds. It’s rather inconvenient when you have to push changes to git, to npm with each release. We will automate this process with GitHub Actions.

![Github Actions](https://miro.medium.com/v2/resize:fit:700/1*RI9j0OJHJ_-WTXYNPliTQg.jpeg)

Github Actions

Create at the root of the project structure the folder `.github`.

Inside it create the folder `workflows`.

Inside `workflows`create the file `publish.yml`:

`.github/workflows/publish.yml`

The main points in the file have been commented.

When we will release it to GitHub, our package will be automatically built and pushed to NPM.

I want you to look at the following line:

_\# We need this to our NPM account  
_NODE\_AUTH\_TOKEN: ${{ secrets.NPM\_TOKEN }}

This is the way we are going to connect GitHub with NPM.

In the browser, open your account on [https://www.npmjs.com/](https://www.npmjs.com/) and go to the page with tokens:

![NPM Access Tokens link](https://miro.medium.com/v2/resize:fit:700/1*PvOLo9KJ91UmbJJlEe66xw.jpeg)

NPM Access Tokens link

Generate a new token:

![Generate new NPM token](https://miro.medium.com/v2/resize:fit:700/1*jMpgfkR6K3GWh5O4bYGALA.jpeg)

Generate new NPM token

You will be prompted to enter its name and select the type. `Automation`is suitable for github-actions:

![New access token](https://miro.medium.com/v2/resize:fit:700/1*eze8nE0Ymx93TINy3szYAQ.jpeg)

New access token

Copy your token and open the github repo.

Go to the tab `Settings`, open `Secrets — Actions`, and create a new repository secret variable. Give it a name `NPM_TOKEN`and paste inside the value of the token from NPM:

![Creating new repository secret variable](https://miro.medium.com/v2/resize:fit:700/1*Zel5lYUu-58W1zt7-UaBYw.jpeg)

Creating new repository secret variable

Well done! Now it remains to create a new version of the project and push it to GitHub.

Before committing any changes, don’t forget to run the following command:

yarn prepublishOnly

Just to make sure that your code will be nice and clean.

After you push the project, go to the Actions tab, where you will see that GitHub has detected that you have workflow actions. But it hasn’t launched anything yet, because we haven’t made a release yet.)

![Github Actions workflows](https://miro.medium.com/v2/resize:fit:700/1*ZV7_FyrSYrmvmA9kWQA_Fg.jpeg)

Github Actions workflows

**Step 12. Demo page**
======================

We have already a folder with an example of using our package, but we would like to create a separate page so that the user can quickly see the package in action without having to clone the entire project. Let’s get on with this page!

We will use github-pages to create the demo.

Create a new git branch:

git checkout -b gh-pages

And now delete all the files from the project!

Don’t worry, all previous files will stay safe in another `master`branch. The branch `gh-pages` will be used only for the demo. That’s why we need to delete all files. Come on, delete everything!

Then in the command line, run:

npm init -y

This command will create the file `package.json`. We will not configure it in detail this time.

For the demo page, I suggest using `parcel` , which is familiar to us ([step 10](#38de))

Run:

yarn add -D parcel my-react-typescript-package react react-dom @types/react-dom @types/react typescript

Create file `.gitignore`:

node\_modules  
yarn.lock  
.parcel-cache  
dist  
yarn-error.log

Create file `tsconfig.json` with the following content:

tsconfig.json

Follow [step 4](#0e1f) to configure the Eslint.

Create empty folder `src` . Inside it, create the file `index.html` with the following content:

src/index.html

Create `index.tsx`:

src/index.tsx

Add to `package.json`inside `scripts`section:

"start": "parcel src/index.html",  
"build": "parcel build src/index.html --dist-dir docs --public-url .",

Take a look at my `package.json`:

package.json

Run the server to make sure everything works fine:

yarn start 

Now, run the code to check for errors using this command:

yarn lint

No errors! Great! It’s time to build the page.

yarn build

You should see the folder `docs` with all files of the demo project.

Here is the structure of the branch `gh-pages`:

![my-react-typescript-package gh-pages, .parcel-cache, dist, docs, index.2eb320be.js, index.html, node_modules library root, src, index.html, index.tsx, .eslintignore, eslintrc, .gitignore, package.json, tsconfig.json, yarn.lock, yarn-error.log](https://miro.medium.com/v2/resize:fit:262/1*pOPI6e-ocPnTLMzj2RNdcg.jpeg)

Structure of the gh-pages branch

Now, let’s push all the changes to git. Run:

git add .  
git commit -m "Initial pages commit"  
git push -u origin gh-pages

Open your GitHub repository, and go to `Settings — Pages`. In the `source`select `/docs`and press `Save:`

![Github pages](https://miro.medium.com/v2/resize:fit:700/1*EnDVUWbn1b8WlNYWEQUFzw.jpeg)

Github pages

Wait about 3–5 minutes and your site will be available on the specified URL, which is shown on your page:

![Link of the demo page](https://miro.medium.com/v2/resize:fit:700/1*eb-mr3oMCPyy50AHWGKdBA.jpeg)

Link to the demo page

We’re done with the demo page. As you see, you can customize it however you want and create a page or website of any complexity.

Here is the result: [https://gapon2401.github.io/my-react-typescript-package/](https://gapon2401.github.io/my-react-typescript-package/)

**Step 13. README.md File and shields.io**
==========================================

Now that we’re done with the code, we can move to the description file of the package.

Switch to `master` branch in git and open `README.md` .

Replace the contents with the following code:

README.md

For the description file, we are using [markdown](https://github.com/adam-p/markdown-here/wiki/Markdown-Here-Cheatsheet).

In `README.md`I have added a short description of the package, link to demo-page, and a usage section.

At the beginning of the readme file, I have small cute icons. This is [https://shields.io/](https://shields.io/.). It is very convenient to have them. I’ve added just a couple as an example, you can use as many as you want.

[Add](https://img.shields.io/github-auth) `shields.io`to your GitHub applications.

Take a look at the end of the file; I’ve created variables with images and links. Markdown allows us to make such variables, so the description file looks more accurate. Don’t forget to change the path to your repo and the package name.

Change version in `package.json` , make commit and push files to the repository. Do everything by yourself. We’ve made it in previous steps.

**Step 14. Making the Release**
===============================

This is the last step. It will be short.

We have done everything we planned! Now we can make an official release on GitHub and also check how automated builds work.

Go to the main page of the repository and click `Create a new release`:

![Create a new release](https://miro.medium.com/v2/resize:fit:700/1*ZnSnaotGzIQ9dHh2nv2Jjw.jpeg)

`Create a new release`

Specify your current version as a tag. It will be created automatically on release. Print release title and description and click `Publish release`.

![Publish release](https://miro.medium.com/v2/resize:fit:700/1*HAesu2BJiJ7Kn84dvwRIXQ.jpeg)

Publish release

Now, open tab `Actions` and make sure your build was successful.

![Automated build on github actions](https://miro.medium.com/v2/resize:fit:700/1*BOXYQW5XvmJJYgmP3bXbLg.jpeg)

Automated build on github actions

Let’s check if the package was updated in NPM.

Open the package link:

https://www.npmjs.com/package/<Your package name>

I will open mine:

[https://www.npmjs.com/package/my-react-typescript-package](https://www.npmjs.com/package/my-react-typescript-package)

You should see a new version with a new description:

![NPM updated package](https://miro.medium.com/v2/resize:fit:700/1*Tlch6DDEbNisynWLl2Qkgw.jpeg)

NPM updated package

It was a long journey, but I hope very useful. I tried to describe everything I had difficulty with when I first published my package and collected this information piece by piece.

Thank you for reading! ❤
