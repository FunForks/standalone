# Harnessing the Power of NodeJS and NPM

You've just levelled up in your journey to be a web developer. You're about to access a whole new level of power.

---

1. _NodeJS_ is the JavaScript engine that is used by Google Chrome, but running as a stand-alone app on your desktop. This means that you can use JavaScript to create code that runs on a server, not in browser.
2. _NPM_ is an acronym for _Node Package Manager_.
3. Node allows you to extend its features using _packages_ or _modules_.
4. The modules that are required for a project are called _dependencies_.
5. There are two main phases in a project: development and production
6. Production modules are used to deliver your project from a server, Development modules are tools that help you build your project (think: the little IKEA key, or a Liebherr crane)
7. You've already been using some modules like _Live-server_, but you've been running this through VS Code. Now you're going to learn how to hand-pick which modules your project needs.
8. You write the custom code (HTML, CSS, JavaScript) for your project: the various generic node modules do all heavy lifting to put your custom online.
9. Your custom code is likely to be much smaller than the node modules used to serve it
10. You will save your custom code on GitHub.
11. You will save a file _listing_ which node modules your project needs on GitHub, but there is no need to fill GitHub with thousands of copies of the same generic files that every project needs.
12. The file that lists the node module is called _package.json_.
13. You can start a new project from an empty folder and a package.json file.
14. You then use `npm install <module_1_name> <module_2_name>` to install production modules.
15. You can install modules that will only be used for development with  `npm install --save-dev <module_1_name> <module_2_name>`.
16. `npm i` is a shortcut for `npm install`.
17. You can also install modules globally using `npm install -g <module_name>`.
18. Modules that are installed globally won't appear in package.json.

---
## Building a Standalone Project from the Ground Up

1. Create a new project folder in a carefully chosen parent directory. Call it "npm-standalone": `d=npm-standalone && mkdir $d && cd $d`
2. Run `npm init -y`.
   * The `-y` flag means "yes: use default values everywhere"
   * This will create a file called `package.json` with the following contents:
   ```json
   {
     "name": "npm-standalone",
     "version": "1.0.0",
     "description": "",
     "main": "index.js",
     "scripts": {
       "test": "echo \"Error: no test specified\" && exit 1"
     },
     "keywords": [],
     "author": "",
     "license": "ISC"
   }
   ```
  * You can edit the `"author"` and `"description"` fields if you want, or you can leave this until later.
3. Rename the `"main"` file to `index.html`, because this is a web project.
4. Create a folder named `src` containing a file named `index.html`:
   ```bash
   mkdir src && code src/index.html
   ```
5. Type `!` and press the `TAB` key to create boilerplate html code
6. Change the `title` of the HTML page, add something to the body such as `<h1>Your web page goes here</h1>`, and save your file.

---
### Install Live-server

1. Run `npm install --save-dev live-server`.
2. Look at your package.json file. It should now have a new entry:
   ```json
   "devDependencies": {
     "live-server": "^1.2.1"
   }
   ```
3. Look at the Explorer pane at the left of your VS Code window. You should see that a folder called `node_modules` has been added. Look inside. It is full of generic third-party files.
4. Get ready to tell Git that you don't want to include any node modules in your repo. Create a (hidden) file called `.gitignore` with the text `node_modules/`:
   ```bash
   echo node_modules/ > .gitignore
   ```
5. Add an instruction to the `scripts` section of your package.json file:
   ```json
   "scripts": {
     "test": "echo \"Error: no test specified\" && exit 1",
     "serve": "live-server src"
   }
   ```
   * Don't worry about the "test" script. It's a placeholder. It doesn't do anything yet. You can delete it.
6. Run the command `npm serve`.
7. This will tell npm to run the `serve` script, which in turn tells Live-server to look for the file defined by `"main"` inside the `src/` folder.
8. You should see a page open in your browser with the headline: "Your web page goes here".

---
### Setting up the Sass Pre-processor

1. Run the command `npm i --save-dev sass`. (Check package.json and node_modules for changes).
2. Create folders for SCSS and CSS in your `src/` folder, and a `main` file inside each of them: 
   ```bash
   code src/scss/main.scss src/styles/main.css
   ```
3. Save these (empty) files
4. While you're at it: check the Auto Save item in VS Code's File menu. This will allow you to see updates happen in real time.
5. Add some SCSS to `src/scss/main.scss`, just for testing purposes:
   ```scss
   $header-color: darkred;
  
   h1 {
     color: $header-color;
   }
   ```
6. Tell Git not to track the `styles/` folder. Add `src/styles/` to `.gitignore`
   * The files is the `scss/` folder will be your source of truth. You are just about to set up the `styles/` folder so that it is updated automatically each time you make a change in the `scss/` folder. For this reason, you need to save the `scss/` folder in Git, but not the derived `styles` folder.
7. Add a new entry to the `scripts` section of your package.json file:
   ```json
   "scripts": {
     "serve": "live-server src",
     "watch": "sass --watch src/scss:src/styles"
   }
   ```
8. In the Terminal, run this command: `npm run watch`.
9. This will tell npm to run the `watch` script, which in turn tells Sass to look for files in the `src/scss/` folder, process them, and write the resulting files out in the `styles/` folder. The `--watch` directive tells Sass to keep checking for changes in any of the files in the `src/scss/` folder, and to re-pre-process all the files whenever a change occurs.
10. Check your `src/styles/main.css` file: It should now contain automatically generated CSS:
   ```css
   h1 {
     color: darkred;
   }

   /*# sourceMappingURL=main.css.map */
   ```

---
### Appling your SCSS to your Web Page

1. Add a link to the head of your index.html page:
   ```html
   <link
     rel="stylesheet"
     href="styles/main.css"
   >
   ```
2. If Live-server is not still active run the command: `npm run serve`.
3. You should see that your headline is now darkred.

---
### Automatic Updates
1. In `src/scss/main.scss` change the color of the `$header-color` variable:
   ```scss
   $header-color: darkgreen;
   ```
2. In your browser, your web page will not update.
3. Run the command `npm i --save-dev npm-run-all`. This installs an npm module that allows you to run two scripts at the same time.
4. Add a new entry to the `scripts` section of your package.json file:
   ```json
   "scripts": {
    "serve": "live-server src",
    "watch": "sass --watch src/scss:src/styles",
    "start": "run-p serve watch"
   }
   ```
7. In the Terminal, run this command: `npm start`.
8. `npm start` is a special command. It is a shortcut for `npm run start`.
9. This command tells the `npm-run-all` module to `run` two scripts in `-p`arallel: `serve` and `watch`.
10. Now, if you change the value for `$header-color` in the `main.scss` file, you should immediately see the changes in your browser.

---
### Create a Git Repository

1. Run `git init`
2. Add all the files in your project (except those listed in `.gitignore`) and make a first commit:
   ```git
   git add . && git commit -m "Barebones standalone project"
   ```
3. Notice that the `node_modules/` and `src/styles/` folders appear in grey (not white) in VS Code's Explorer pane. This indicates that Git is not tracking them. 

### Push your Repository to GitHub

In the past, you started by creating a repository on GitHub, and then you cloned it to your local computer. Here you will learn how to connect your existing repo on your local computer to a repository on GitHub.

1. Visit your account on GitHub.
2. Select the Repositories tab.
3. Click on the green New button.
4. Give your new repository a name. I'll use "Deploy-Test", but you can use whatever name you want. 
5. Leave all the radiobuttons and checkboxes at their default values.
6. Click on the green Create Repository button.
7. In the second section of the Quick Setup, copy the three lines that appear after the sub-header **...or push an existing repository...**:
   '''bash
   git remote add origin git@github.com:<yourAccountName>/<yourRepoName>.git
   git branch -M main
   git push -u origin main
   ```
8. In a Terminal pane in the VS Code window which is open in your project folder, paste these lines and press Enter.
9. Run `git remote -v` to check that a remote named "origin" has been created.
10. In your browser, refresh the GitHub page. You should see a list of your files and folders.

---
# Deploy to GitHub Pages

Up until now, you have been working in a development environment, serving your web pages with Live-Server from your local computer. Only you (on your computer) and other people on your local network can see your pages.

To share your work with the world, you need to _deploy_ to a production server. You can do this very simply on [GitHub.com](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site), with a few point-and-click actions.

You can also use a node module called `gh-pages` (for GitHub Pages), to do this directly from your local repository, using a local Terminal window.


## Installing the `gh-pages` module

1. Run `npm i --save-dev gh-pages`.
2. Look in `package.json` to see that it has been installed:
   ```bash
   "devDependencies": {
     "gh-pages": "latest",
     "live-server": "latest",
     "npm-run-all": "latest",
     "sass": "latest"
   }
   ```
   Alternatively, you can run `npm list` to see what node modules have been installed:
   ```bash
   $ npm list
   npm-standalone@1.0.0 /Users/james/Documents/DCI/Repos/DCIForks/   SASS/npm-standalone
   ├── gh-pages@3.2.3
   ├── live-server@1.2.1
   ├── npm-run-all@4.1.5
   └── sass@1.49.9
   ```

## Creating a Production Build

When you are developing a project, the code you are editing and adding to is not yet optimized. When everything is working correctly, you want to remove all the extra data and tools that you used during development.

When builders finish building a house, they take away all the cranes and scaffolding, bulldozers, concrete mixers, power tools and protective plastic sheeting, to leave the house pleasant for the people who are going to live in it.

The code and data that the outside world sees is the _production build_ of your web site. Fortunately, there are modules that are specifically designed for cleaning out your work.

In this section, you will see how to:

* Remove any previous production build
* Delete any current CSS files
* Generate a new set of up-to-date CSS files
* Create a new production build from the current `src/` folder (omitting any SCSS files which are only needed during development)
* Create a special `deploy` branch for the production build
* Push the `deploy` branch to your account on GitHub.io (note the `.io` top level domain here)

You will then be able to give a URL with the format [https://yourAccountName.github.io/yourRepositoryName]() to anyone who might be interested: fellow students, friends, future employers, end users, clients, ...

Your site will be live.

### Creating a `dist` directory

1. In the Terminal you can run the following lines of code:
   ```bash
   mkdir dist
   rsync -av --exclude="/scss" src/ dist
   ```
   * The first line creates a directory called `dist`.
   * The second line uses a bash tool call [rsync](https://linux.die.net/man/1/rsync) to copy all the files from the `src` folder to the new `dist` folder... all except for the `scss/` folder which contains CSS in a format that browsers do not understand. The `styles/` folder containing regular CSS will be copied instead.
   * The `-av` flags mean:
     -a: archive (preserve all ownership, permissions and metadata, recursively)
     -v: verbose (report on which files were copied)
2. You should see that a new `dist/` folder has appeared, with the contents copied from the `src/` folder:
   ```
   dist
   ├── index.html
   └── styles
       ├── main.css
       └── main.css.map
   ```
3. Because of the `-v` flag, you should see something like this in the Terminal pane:
   ```   
   building file list ... done
   ./
   index.html
   styles/
   styles/main.css
   styles/main.css.map
   ```
   * You can check that all the files in the `src/` folder and their subfolders have been copied, apart from `scss/` and its contents.

### Creating a "publish" script

1. Add a new entry to the `scripts` section of your package.json file:
   ```json
   "scripts": {
    "serve": "live-server src",
    "watch": "sass --watch src/scss:src/styles",
    "start": "run-p serve watch",
    "publish": "gh-pages -d dist"
   }
   ```
2. Now you can execute the command `npm run publish`.
3. In your browser, visit the page [https://yourAccountName.github.io/Deploy-Test]()
4. You will probably see a 404 error. This is normal. Many people are pushing data to GitHub and deploying their sites at the same time. Your deploy request will have been added to a queue and will be treated within a few minutes.
5. Refresh the page every minute or so, and eventually you should see:
   **Your web site goes here**

## Automating the Deployment Process

You can reduce the deployment process to a single command in the Terminal: `npm run deploy`. To do this, you will need to add a number of entries to the `"scripts"` section of your package.json file.

1. First, you should remove the existing `dist/` folder. A little later, you will recreate the folder with the latest data from the `src/` folder.
   * To delete a folder and all its contents, you already know the `rm -rf <folderName>` bash command.
   * `rm` means "remove"
   * `-rf` means "recursively" (remove all contents from all sub-folders) and "forced" (don't ask to confirm each deletion.)

2. Here's a script to add to the `"scripts"` section of your package.json file which will remove the `dist/` folder:
    ```bash
    "clean": "rm -rf dist",
    ```
3. And here's one to delete the `src/styles/` folder:
   ```bash
   "clean:styles": "rm -rf src/styles",
   ```

4. Now you want to recreate the `src/styles` folder from the latest version of whatever is in the `src/scss/` folder. Note that you don't want to `--watch` for changes; you want to apply the current SCSS.
5. Here's a new script to add to the `"scripts"` section of your package.json file which will do this:
   ```bash
   "build:styles": "sass src/scss:src/styles",
   ```
6. One last step: you want to recreate the `dist/` folder. You can use the same command you used manually earlier:
   ```bash
   "copy": "mkdir dist && rsync  --exclude=\"/scss\" src/ dist",
   ```
   * Note 1: `&&` means "if the first command succeeds (i.e. `mkdir dist`), then perform the second command (i.e. `rsync ...`). If not, stop here."
   * Note 2: The double-quotes in `"/scss"` need to be _escaped_, because the whole command is wrapped in double-quotes. If the anti-slash before the quotes around `\"scss/\"` were removed, npm would think that the command was `"mkdir dist && rsync  --exclude="`, followed by some garbage. The escape `\` characters mean "treat the next character as part of this string, not as the end of the string."

7. Now we can make a `"build"` script which will run all these new scripts. It will use the `npm-run-all` module to execute each script `-s`equentially (one after another):
   ```bash
   "build": "run-s clean clean:styles build:styles copy",
   ```
8. You've already created a `"publish"` script that uses the `gh-pages` module to push your work to GitHub. Here it is as a reminder:
   ```bash
   "publish": "gh-pages -d dist",
   ```

9. So now you need just one last script that makes all the others run sequentially, ending with `"publish"`:
   ```bash
   "deploy": "run-s build publish"

### The Complete `scripts` Section

```json
"scripts": {
  "serve": "live-server src",
  "watch": "sass --watch src/scss:src/styles",
  "start": "run-p serve watch",

  "clean": "rm -rf dist",
  "clean:styles": "rm -rf src/styles",
  "build:styles": "sass src/scss:src/styles",
  "copy": "mkdir dist && rsync  --exclude=\"/scss\" src/ dist",
  "build": "run-s clean clean:styles build:styles copy",
  "publish": "gh-pages -d dist",
  "deploy": "run-s build publish"
}
```
Now you can make any changes you like to your project locally, and, when you are ready to go live, all you will need to type is:

  ```bash
  npm run deploy
  ```