This is where Beyond Z participants login and access their leadership development portal.


# Getting Started


Follow this tutorial to get Rails setup for Heroku:
https://devcenter.heroku.com/articles/getting-started-with-rails4

You must install Postgres (http://postgresapp.com/)
and set your PATH in your ~/.bashrc:

	export PATH="/Applications/Postgres93.app/Contents/MacOS/bin:$PATH"

After your environment is setup, fork this repository on Github. Then in the location you want the local copy, run:

	git clone <your_forked_url>

To get all your gems, run:

	bundle install

## Configuration

In the "/env.sample" file is a list of environment variables that must be set for your database, SMTP, etc... to function properly. Copy this file to ".env"  and be sure these values are set for your Rails environment using your Foreman, Pow or preferred server setup.

### Creating a new Secret Token

	rake secret
	
Take the results of the above command and put this in your ".env" file for RAILS\_SECRET\_TOKEN.

## Running the Application 
From your repo directory:

	rake db:create
	rake db:migrate

And to start website on local machine, run: $foreman start and the app will be available at http://localhost:3000

## Code Management

Here is a nice description of the workflow we follow, which is also
detailed below:
http://nathanhoad.net/git-workflow-forks-remotes-and-pull-requests 

### Setup
To move on with development, run:

	git remote add upstream https://github.com/beyond-z/beyondz-platform.git
	
	
This makes the upstream (original repo) code available for merging into your local fork.

### Flow

Always create a new branch when working on a new feature. From your local master branch:

	git checkout -b <feature_name>

To commit all changes:

	git commit -am 'a brief message saying what you did. think about future readers.'

To push your local changes to your Github fork, run the static code analysis lint tool 
and the tests like this:

	rubocop .
	rake test
	git push origin <feature_name>

To submit a pull request and integrate your changes back to the main
repository do the following:

Select the feature branch from your Github page using the drop down selector. 

![Select Branch](docs/select-branch.png)


Then click the green pull request button to the left hand side of the drop down.

On the next screen, click "Edit" near the right-hand side of the screen.

![Edit location](docs/edit-branch.png)

Then choose the 'staging' branch of the beyondz-platform. 

![Switch to staging](docs/staging-pull.png)

Write a meaningful title and summary so it is well documented what this "feature" 
is when looking back or at a glance.  Your pull request will be rejected if the 
title and  summary is cryptic for other readers.

Once the pull is merged, do some cleanup on your local branch:

* Stay up to date by merging the staging repository back to your
local branch.
```
	git pull upstream staging
```

* Switch back to staging (or some other branch) and delete the feature
branch (locally and remotely)
```
  git checkout staging
  git branch -d <feature_name>
  git push origin :<feature_name>
```

### Continuous Integration
We use a continuous integration test server on all pull requests. When you
open a pull request, it will be automatically tested and the results displayed
on GitHub in the form of a checkbox or an X mark.

The current integration runs the test suite as well as rubocop. Any errors resulting from either will show as a failure.

You can see the details here: https://travis-ci.org/beyond-z/beyondz-platform

# Coding Conventions

## Ruby/Rails

We use standard Rails code conventions with some additional rules:

  * Indent each level with two spaces
  * Always raise subclasses of Exception specialized to your need, and always rescue a specific type.
  * Always use Rails database migrations when adding new data.
  * Write the main class at the top of the file. Try to stick to one class per file, but a small helper (e.g. an exception subtype) may appear below the main class.
  * Always use begin, raise, and rescue for error handling. Don't use throw and catch in Ruby.
  * Keep individual lines simple. If a new reader can't immediately tell what it is doing, either simplify the code or refactor it into a named method.
  * Use the flash hash to quick message workflows.
  * Never commit a FIXME: either fix it or make a task in Asana.

This is the full style guide we adhere to: https://github.com/bbatsov/ruby-style-guide

Remember to run rubocop before submitting pull requests to help keep code up to standards.

## CSS

Structure CSS files according to the ![asset management guide](app/assets/asset_management_guide.pdf).

  * Avoid placing CSS in view files.
  * Use dashes in class/id names, not underscores.
  * Beginning curly brace should be on the same line as the class name.
  * Ending curly brace should be vertically inline with the class name.
  * Use empty lines between class definitions.
  * Use Bootstrap styles and components (CSS and JS) whenever possible.
  * Whenever possible, avoid using Bootstrap classes directly in view files. Instead, create a class that extends Bootstrap classes.
  * Utilize SASS, but minimize nesting. Be aware of bloat.
  * Consider refactoring and generalizing styles into the asset management structure to maximize reuse.
  * Choose semantic concepts for styles over those that are page specific (e.g. .contact-us-header), mapped to HTML structures (e.g. .contact-table-row), or style descriptions (e.g. .green-button).
  * When possible use CSS selectors that address tags (input[type=submit]) instead of custom names (.submit-button).
  * All styles should be properly scoped so that generic classes like ".document" or ".form" don't accidentally override other styles. Instead use something like ".comment .document" or ".comment form" to limit their application.

## JavaScript

Structure JS files according to the ![asset management guide](app/assets/asset_management_guide.pdf).

  * Avoid placing JS in view files.
  * Use Bootstrap components wherever possible.
  * Use JQuery for additional components or to add interactivity, etc...
  * Use inline curly braces.
  * Properly scope selectors so to avoid side effects on other elements.
  * Reuse selector variables wherever possible. No need to continually reselect the same HTML elements.