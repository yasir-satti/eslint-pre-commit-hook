# Add Linting with Prettier and Pre-Commit hook to React project


Reference Tutorial with a few tweaks... 



## Start with Create React App using TypeScript template


	$ npx create-react-app aslant-pre-commit-hook --template typescript

## Add the following script to "package.json" to make type checking a bit easier:

	"typescript": "tsc --project tsconfig.json --noEmit"



## Github Project repository

### Create project repository

### Initialise locale project directory to git repo using 'git init'

### Push project to GitHub repo


## Run app localley and check tests


## Eslint

### Install ESlint

	$ npm install eslint --save-dev

### Initialise ESlint configuration

	$ npm init @eslint/config

Follow the configuration steps with the following answers:

	A- To check syntax and find problems
	B- JavaScript modules (import/export)
	C- React
	D- Yes
	E- Browser
	F- JSON
	G- Yes
	H- npm

.eslintrc.json file is created in project root


### Add file ".eslintignore" on project root with this content

	build
	node_modules
	tsconfig.json


### Add lint and lint:fix scripts to "package.json"

	"lint": "eslint src --ext .ts,.tsx",
	"lint:fix": "eslint src --ext .ts,.tsx --fix",


## Prettier

### Install Prettier

	$ npm install prettier --save-dev



### Create a basic config file, ".prettierrc.json" , at the root of the project

{
    "printWidth": 80,
    "trailingComma": "all",
    "tabWidth": 4,
    "semi": true,
    "singleQuote": true,
    "arrowParens": "avoid",
	"useTabs": true
}



### Install ESlint plugins to work with Prettier

	$ npm i --save-dev eslint-config-prettier eslint-plugin-prettier@latest



### Add Prettier to ESlint configuration in ".eslintrc.json" file

	"extends": [
        "plugin:react/recommended",
        "standard-with-typescript",
        "prettier"
    ],
    "plugins": [
        "@typescript-eslint",
        "react",
        "prettier"
    ],


### Add this rule to "rules" section in ".eslintrc.json" file

	"prettier/prettier": ["error", { "endOfLine": "auto" }]


Now try lint script

	$ npm run lint



## Pre-Commit Hook using Husky


### Install Husky and lint-staged:

	$ npm install husky lint-staged --save-dev



### Install tsc-files to ensure we can only check the types of staged files:

	$ npm install tsc-files --save-dev



### Add lint-staged config, defining the necessary checks to "lint-staged.js" file at the root of project

	export default {
    	'*.{js,jsx,ts,tsx}': [
        	'eslint --max-warnings=0', 
        	'react-scripts test --bail --watchAll=false --findRelatedTests --passWithNoTests',
        	() => 'tsc-files --noEmit',
    	],
    	'*.{js,jsx,ts,tsx,json,css,js}': ['prettier --write'],
	}


### Add this line in "package.json" file before dependencies section at the top of the file

	"type": "module",



### Add add a script for lint-staged in "package.json":

	"lint-staged": "lint-staged --config lint-staged.js",



### Set up the pre-commit hook, create a script in "package.json":

	"husky-install": "husky install",


Run it with:

	$ npm run husky-install


### Create hook with the following command:

	$ npx husky add .husky/pre-commit "npm run lint-staged"



The following file will be automatically generated in the ".husky" folder at the root of the project:

	#!/bin/sh
	. "$(dirname "$0")/_/husky.sh"
	npm run lint-staged


From now on, the checks defined in the lint-staged config will run on every commit



## Let us try them all when trying to commit changes 

Break linting with changing some formating

Also break the test with change to the search text

Stage your changes

Try to commit your changes

The pre-commit hook will run and you should have linting error and test broken messages

You need to fix them, then stage the changes again and try commit them. The pre-commit hook should run with no errors. Now you can push your changes.
