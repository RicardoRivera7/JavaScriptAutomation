


<h1>Main Test</h1>

```


const { test } = require('@playwright/test');
const { LoginPage } = require('../pages/login');
const { BoardPage } = require('../pages/boardpage');
const testData = require('../testdata/boardtestdata.json');

test.describe('Data Driven Board Tests - JavaScript', () => {

  test.beforeEach(async ({ page }) => {
    const loginPage = new LoginPage(page);
    await loginPage.goto();
    await loginPage.login('admin', 'password123');
  });

  testData.forEach((scenario) => {

    test(`Validate task: ${scenario.task}`, async ({ page }) => {

      const boardPage = new BoardPage(page);

      await boardPage.navigateToProject(scenario.project);
      await boardPage.verifyColumnandTask(scenario.column, scenario.task);
      await boardPage.verifyTags(scenario.task,scenario.tags);

    });

  });

});



```


<br/>


<h1>Login Page</h1>


```

class LoginPage {
  constructor(page) {
    this.page = page;
    this.usernameInput = page.locator('xpath=//input[@id="username"]');
    this.passwordInput = page.locator('xpath=//input[@id="password"]'); 
  }

  async goto() {
    await this.page.goto('/');  
   
  }

  async login(username, password) {
    await this.usernameInput.fill(username);
    await this.passwordInput.fill(password);
    await this.page.click('button[type="submit"]');
  }
}

module.exports = { LoginPage };


```


<br/>


<h1>Board Page</h1>

```

const { expect } = require('@playwright/test');

class BoardPage {
  constructor(page) {
    this.page = page;
  }

  async navigateToProject(projectName) {
    await this.page.click(`text=${projectName}`);
  }

  // Verifies the task name is under the correct Column name (Ex: To Do || In Progress)
  async verifyColumnandTask(columnName, taskName) {
     const column = this.page.locator('div.flex.flex-col.w-80.bg-gray-50.rounded-lg.p-4', {has: this.page.locator('h2', { hasText: columnName })});

  await expect(column).toHaveCount(1);  //ensures it grabs the unique column    
  await expect(column.locator(`h2`, { hasText: columnName })).toBeVisible(); //verifes name of column matches expected name

 
  const task = column.locator('h3', { hasText: taskName }); //verifies it finds the task name within the column
  await expect(task).toHaveCount(1);    // ensure exactly one unique task name
  await expect(task).toBeVisible(); //checks if task is visible on screen to user
  }


  async verifyTags(taskName, tags) {

   const taskCard = this.page.locator('div.bg-white.p-4.rounded-lg.shadow-sm.border', { has: this.page.locator('h3', { hasText: taskName })}); //finds the title of the task
   await expect(taskCard).toHaveCount(1); //ensures no duplicates of task cards

   const getTags = await taskCard.locator('div.flex.flex-wrap span').allTextContents(); //gets all tags
   const trimmedTags = getTags.map(t => t.trim()); //separates all tags into array

   expect(trimmedTags.sort()).toEqual(tags.sort()); //compares grabbed tags to expected tags of json

    
  }
}

module.exports = { BoardPage };





```


<br/>
<h1>Board Page Json</h1>

```
[
  {
    "project": "Web Application",
    "task": "Implement user authentication",
    "column": "To Do",
    "tags": ["Feature", "High Priority"]
  },
  {
    "project": "Web Application",
    "task": "Fix navigation bug",
    "column": "To Do",
    "tags": ["Bug"]
  },
  {
    "project": "Web Application",
    "task": "Design system updates",
    "column": "In Progress",
    "tags": ["Design"]
  },
  {
    "project": "Mobile Application",
    "task": "Push notification system",
    "column": "To Do",
    "tags": ["Feature"]
  },
  {
    "project": "Mobile Application",
    "task": "Offline mode",
    "column": "In Progress",
    "tags": ["Feature", "High Priority"]
  },
  {
    "project": "Mobile Application",
    "task": "App icon design",
    "column": "Done",
    "tags": ["Design"]
  }
]




```

<br/>
<h1>Config</h1>

```

const { defineConfig } = require('@playwright/test');

module.exports = defineConfig({
  testDir: './tests',
  use: {
    baseURL: 'https://animated-gingersnap-8cf7f2.netlify.app/',
    headless: false,
    viewport: { width: 1280, height: 720 }
  }
});


```


}
