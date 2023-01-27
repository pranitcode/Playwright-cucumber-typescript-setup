# Playwright-cucumber-typescript-setup
## Playwright-Cucumber-Typescript-Setup


- Open new folder in vscode and then open terminal of vscode
- In terminal type first command

```
$ npm init -y // it will install  pacakaj.json file

$ npm init playwright@latest // choose typescript

$ npm i -D @cucumber/cucumber@7.3.1 @cucumber/pretty-formatter

$ npm i playwright @cucumber/cucumber typescript ts-node @types/node -D

$ npx -p typescript tsc --init 
```
- Now open tsconfig.json file and write below code

```

  +  "ts-node": {
  +    "transpileOnly": true
  + },

  "compilerOptions": {   
```

### Create two new folder  

  - features
  - step-definitions
  
### Now inside *features* folder create new file

   - Assignment.feature

- After this open file  *Assignment.feature* write below code

```
Feature:spicejet ticket booking 
Scenario:validate spicejet booking portals

Given I navigate to spicejet url

```



   
### Then inside step-definitions folder create new file

   - stepElement.ts

- Now open file *stepElement.ts* write below code in that file

```

import { Given, When, Then } from "@cucumber/cucumber";
import { expect } from "@playwright/test"; 
import { page } from '../test.setup'; // check path properly // this exported from test.setup.ts 


Given("I navigate to spicejet url", async function () {
  await page.goto(process.env.SPICEJET);
});

```
*******************************



Now create one another file 

- test.setup.ts // for cucmber config
- inside test.setup.ts   file write below code // if you have already file called *cucumber.conf.js* you can rename it to *test.setup.ts*  compare it with below code (do chages if needed)

```
import  { Before, BeforeAll, AfterAll, After, setDefaultTimeout } from "@cucumber/cucumber";
import { chromium , Browser, BrowserContext , Page}  from "playwright";

let browser: Browser;
let page: Page;
let context: BrowserContext;

setDefaultTimeout(60000)

// launch the browser
BeforeAll(async function () {
    browser = await chromium.launch({
        headless: false,
        slowMo: 1000,
    });
});

 // close the browser
 AfterAll(async function () {
    await browser.close();
 });
 
 // Create a new browser context and page per scenario
Before(async function () {
    context = await browser.newContext({permissions: []});
    page = await context.newPage();
 });
 
 // Cleanup after each scenario
 After(async function () {
    await page.close();
    await context.close();
 });

export {page, browser} // this page will used in stepElement.ts file

```




### Inside package.json file add below script

```

  "scripts": {

    "test": "cucumber-js features/**/*.feature --require-module ts-node/register --require test.setup.ts --require step-definitions/**/*.ts"

  },

```

Now setup is completed for runing it used below command 
```

$ npm run test features/Assignment.feature 

```

if you find error check file name and script test in package.json file 

     
