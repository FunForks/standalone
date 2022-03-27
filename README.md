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
2. If Live-server is not still active un the command: `npm run serve`.
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