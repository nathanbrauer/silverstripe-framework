title: Contributing Code
summary: Fix bugs and add new features to help make SilverStripe better.

# Contributing Code - Submitting Bugfixes and Enhancements

SilverStripe will never be finished, and we need your help to keep making it better.  If you're a developer a great way to get involved is to contribute patches to our modules and core codebase, fixing bugs or adding features.

The SilverStripe core modules (`framework` and `cms`), as well as some of the more popular modules are in
git version control. SilverStripe hosts its modules on [github.com/silverstripe](http://github.com/silverstripe) and [github.com/silverstripe-labs](http://github.com/silverstripe-labs).  After [installing git](http://help.github.com/git-installation-redirect) and creating a [free github.com account](https://github.com/signup/free), you can "fork" a module,
which creates a copy that you can commit to (see github's [guide to "forking"](http://help.github.com/forking/)).

For other modules, our [add-ons site](http://addons.silverstripe.org/add-ons) lists the repository locations, typically using the version control system like "git".

<div class="hint" markdown="1">
Note: By supplying code to the SilverStripe core team in patches, tickets and pull requests, you agree to assign copyright of that code to SilverStripe Limited, on the condition that SilverStripe Limited releases that code under the BSD license.

We ask for this so that the ownership in the license is clear and unambiguous, and so that community involvement doesn't stop us from being able to continue supporting these projects.  By releasing this code under a permissive license, this copyright assignment won't prevent you from using the code in any way you see fit.
</div>

## Step-by-step: From forking to sending the pull request

<div class="notice" markdown='1'>
**Note:** Please adjust the commands below to the version of SilverStripe that you're targeting.
</div>

1. Install the project through composer. The process is described in detail in "[Installation through Composer](../getting_started/composer#contributing)".

 		composer create-project --keep-vcs --dev silverstripe/installer ./my/website/folder 4.0.x-dev

2. Edit the `composer.json`. Remove any `@stable` markers from the core modules in there. 
   Add your fork URLs, in this example a fork of the `cms` module on the `sminnee` github account 
   (replace with your own fork URL). Run a `composer update` afterwards.

		"repositories": [
			{
				"type": "vcs",
				"url": "git@github.com:sminnee/silverstripe-cms.git"
			}
		],

3. Add a new "upstream" remote so you can track the original repository for changes, and rebase/merge your fork as required.

		cd cms
		git remote add -f upstream git://github.com/silverstripe/silverstripe-cms.git

4. [Branch for new issue and develop on issue branch](code#branch-for-new-issue-and-develop-on-issue-branch)

		# verify current branch 'base' then branch and switch
		git status
		git branch ###-description
		git checkout ###-description

5. As time passes, the upstream repository accumulates new commits. Keep your working copy's branch and issue branch up to date by periodically [rebasing your development branch on the latest upstream](code#rebase-your-development-branch-on-the-latest-upstream).

		# [make sure all your changes are committed as necessary in branch]
		git fetch upstream
		git rebase upstream/3.2

6. When development is complete, [squash all commit related to a single issue into a single commit](code#squash-all-commits-related-to-a-single-issue-into-a-single-commit).

		git fetch upstream
		git rebase -i upstream/3.2

7. Push release candidate branch to GitHub 

		git push origin ###-description

8. Issue pull request on GitHub.  Visit your forked repository on GitHub.com and click the "Create Pull Request" button next to the new branch.

The core team is then responsible for reviewing patches and deciding if they will make it into core.  If
there are any problems they will follow up with you, so please ensure they have a way to contact you!

### The Pull Request Process

Once your pull request is issued, it's not the end of the road. A [core committer](/contributing/core_committers/) will most likely have some questions for you and may ask you to make some changes depending on discussions you have.
If you've been naughty and not adhered to the coding conventions, expect a few requests to make changes so your code is in-line.

If your change is particularly significant, it may be referred to the [mailing list](https://groups.google.com/forum/#!forum/silverstripe-dev) for further community discussion.

A core committer will also "label" your PR using the labels defined in GitHub, these are to correctly classify and help find your work at a later date.

#### GitHub Labels

The current GitHub labels are grouped into 5 sections:

1. *Changes* - These are designed to signal what kind of change they are and how they fit into the [Semantic Versioning](http://semver.org/) schema
2. *Impact* - What impact does this bug/issue/fix have, does it break a feature completely, is it just a side effect or is it trivial and not a bit problem (but a bit annoying)
3. *Effort* - How much effort is required to fix this issue?
4. *Type* - What aspect of the system the PR/issue covers
5. *Feedback* - Are we waiting on feedback, if so who from? Typically used for issues that are likely to take a while to have feedback given

| Label | Purpose |
| ----- | ------- |
| change/major | A change for the next major release (eg: 4.0) |
| change/minor | A change for the next minor release (eg: 3.x) |
| change/patch | A change for the next patch release (eg: 3.1.x) |
| impact/critical | Broken functionality for which no work around can be produced |
| impact/high | Broken functionality but can be mitigated by other non-core code changes |
| impact/medium | Unexpected behaviour but does not break functionality |
| impact/low | A nuisance but doesn't break any functionality (typos, etc) |
| effort/easy | Someone with limited SilverStripe experience could resolve |
| effort/medium | Someone with a good understanding of SilverStripe could resolve |
| effort/hard | Only an expert with SilverStripe could resolve |
| type/docs | A docs change |
| type/frontend | A change to front-end (CSS, HTML, etc) |
| feedback-required/core-team | Core team members need to give an in-depth consideration |
| feedback-required/author | This issue is awaiting feedback from the original author of the PR |

### Workflow Diagram

[![Workflow diagram](http://www.silverstripe.org/assets/doc-silverstripe-org/collaboration-on-github.png)](http://www.silverstripe.org/assets/doc-silverstripe-org/collaboration-on-github.png)

### Quickfire Do's and Don't's

If you aren't familiar with git and GitHub, try reading the ["GitHub bootcamp documentation"](http://help.github.com/). 
We also found the [free online git book](http://git-scm.com/book/) and the [git crash course](http://gitref.org/) useful.
If you're familiar with it, here's the short version of what you need to know. Once you fork and download the code:

* **Don't develop on the master branch.** Always create a development branch specific to "the issue" you're working on (on our [GitHub repository's issues](https://github.com/silverstripe/silverstripe-framework/issues)). Name it by issue number and description. For example, if you're working on Issue #100, a `DataObject::get_one()` bugfix, your development branch should be called 100-dataobject-get-one. If you decide to work on another issue mid-stream, create a new branch for that issue--don't work on both in one branch.

* **Do not merge the upstream master** with your development branch; *rebase* your branch on top of the upstream master.

* **A single development branch should represent changes related to a single issue.** If you decide to work on another issue, create another branch.

* **Squash your commits, so that each commit addresses a single issue.** After you rebase your work on top of the upstream master, you can squash multiple commits into one. Say, for instance, you've got three commits in related to Issue #100. Squash all three into one with the message "Description of the issue here (fixes #100)" We won't accept pull requests for multiple commits related to a single issue; it's up to you to squash and clean your commit tree. (Remember, if you squash commits you've already pushed to GitHub, you won't be able to push that same branch again. Create a new local branch, squash, and push the new squashed branch.)

* **Choose the correct branch**: Assume the current release is 3.0.3, and 3.1.0 is in beta state.
Most pull requests should go against the `3.1` *pre-release branch*, only critical bugfixes
against the `3.0` *release branch*. If you're changing an API or introducing a major feature,
the pull request should go against `master` (read more about our [release process](release_process)). Branches are periodically merged "upwards" (3.0 into 3.1, 3.1 into master).

### Editing files directly on GitHub.com

If you see a typo or another small fix that needs to be made, and you don't have an installation set up for contributions, you can edit files directly in the github.com web interface.  Every file view has an "edit this file" link.

After you have edited the file, GitHub will offer to create a pull request for you.  This pull request will be reviewed along with other pull requests.

## Check List

* Adhere to our [coding conventions](/getting_started/coding_conventions)
* If your patch is extensive, discuss it first on the [silverstripe-dev google group](https://groups.google.com/group/silverstripe-dev) (ideally before doing any serious coding)
* When working on existing tickets, provide status updates through ticket comments
* Check your patches against the "master" branch, as well as the latest release branch
* Write [unit tests](../developer_guides/testing/unit_testing)
* Write [Behat integration tests](https://github.com/silverstripe-labs/silverstripe-behat-extension) for any interface changes
* Describe specifics on how to test the effects of the patch
* It's better to submit multiple patches with separate bits of functionality than a big patch containing lots of changes
* Only submit a pull request for work you expect to be ready to merge. Work in progress is best discussed in an issue, or on your own repository fork.
* Document your code inline through [PHPDoc](http://en.wikipedia.org/wiki/PHPDoc) syntax. See our 
[API documentation](http://api.silverstripe.org/3.1/) for good examples.
* Check and update documentation on [docs.silverstripe.org](http://docs.silverstripe.org). Check for any references to functionality deprecated or extended through your patch. Documentation changes should be included in the patch.
* When introducing something "noteworthy" (new feature, API change), [update the release changelog](/changelogs) for the next release this commit will be included in.
* If you get stuck, please post to the [forum](http://silverstripe.org/forum) or for deeper core problems, to the [core mailinglist](https://groups.google.com/forum/#!forum/silverstripe-dev)
* When working with the CMS, please read the ["CMS Architecture Guide"](/developer_guides/customising_the_admin_interface/cms_architecture) first

## Commit Messages

We try to maintain a consistent record of descriptive commit messages. 
Most importantly: Keep the first line short, and add more detail below.
This ensures commits are easy to browse, and look nice on github.com
(more info about [proper git commit messages](http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html)).

As we automatically generate [changelogs](http://doc.silverstripe.org/sapphire/en/trunk/changelogs/) from them, we need a way to categorize and filter. 
Please prefix **noteworthy** commit messages with one of the following tags: 

* `NEW` New feature or major enhancement (both for users and developers)
* `API` Addition of a new API, or modification/removal/deprecation of an existing API. Includes any change developers should be aware of when upgrading.
* `BUG` Bugfix or minor enhancement on something developers or users are likely to encounter.

All other commits should not be tagged if they are so trivial that most developers
can ignore them during upgrades or when reviewing changes to the codebase.
For example, adding unit tests or documentation would not be considered "noteworthy".
Same goes for version control plumbing like merges, file renames or reverts.

Further guidelines:

* Each commit should form a logical unit - if you fix two unrelated bugs, commit each one separately
* If you are fixing a issue from our bugtracker ([cms](http://github.com/silverstripe/silverstripe-framework) and [framework](http://github.com/silverstripe/silverstripe-framework)), please append `(fixes #<ticketnumber>)`
* When fixing issues across repos (e.g. a commit to `framework` fixes an issue raised in the `cms` bugtracker),
  use `(fixes silverstripe/silverstripe-cms#<issue-number>)` ([details](https://github.com/blog/1439-closing-issues-across-repositories))
* If your change is related to another commit, reference it with its abbreviated commit hash. 
* Mention important changed classes and methods in the commit summary.

Example: Bad commit message

	finally fixed this dumb rendering bug that Joe talked about ... LOL
	also added another form field for password validation

Example: Good commit message

	BUG Formatting through prepValueForDB() 

	Added prepValueForDB() which is called on DBField->writeToManipulation() 
	to ensure formatting of value before insertion to DB on a per-DBField type basis (fixes #1234).
	Added documentation for DBField->writeToManipulation() (related to a4bd42fd).

## The steps in more detail

### Branch for new issue and develop on issue branch

Before you start working on a new feature or bugfix, create a new branch dedicated to that one change named by issue number and description. If you're working on Issue #100, a `DataObject::get_one()` bugfix, create a new branch with the issue number and description, like this:

	$ git checkout -b 100-dataobject-get-one

Edit and test the files on your development environment. When you've got something the way you want and established that it works, commit the changes to your branch on your local git repo. 

	$ git add <filename>
	$ git commit -m 'Some kind of descriptive message (fixes #100)' 

You'll need to use git add for each file that you created or modified. There are ways to add multiple files, but I highly recommend a more deliberate approach unless you know what you're doing.

Then, you can push your new branch to GitHub, like this (replace `100-dataobject-get-one` with your branch name):

	$ git push origin 100-dataobject-get-one

You should be able to log into your GitHub account, switch to the branch, and see that your changes have been committed. Then click the Pull button to request that your commits get merged into the development master.  

### Rebase Your Development Branch on the Latest Upstream

To keep your development branch up to date, rebase your changes on top of the current state of the upstream master. See the [What is git rebase?](code#what-is-git-rebase) section below to learn more about rebasing.

If you've set up an upstream branch as detailed above, and a development branch called `100-dataobject-get-one`, you can update `upstream` and rebase your branch from it like so:

	# make sure all your changes are committed as necessary in branch
	$ git fetch upstream
	$ git rebase upstream/master

Note that the example doesn't keep your own master branch up to date.  If you wanted to that, you might take the following approach instead:

	# make sure all your changes are committed as necessary in branch
	$ git fetch upstream
	$ git checkout master
	$ git rebase upstream/master
	$ git checkout 100-dataobject-get-one
	$ git rebase master

You may need to resolve conflicts that occur when a file on the development trunk and one of your files have both been changed. Edit each file to resolve the differences, then commit the fixes to your development server repo and test. Each file will need to be "added" before running a "commit." 

Conflicts are clearly marked in the code files. Make sure to take time in determining what version of the conflict you want to keep and what you want to discard. 

	$ git add <filename>
	$ git rebase --continue

### Squash All Commits Related to a Single Issue into a Single Commit

Once you have rebased your work on top of the latest state of the upstream master, you may have several commits related to the issue you were working on. Once everything is done, squash them into a single commit with a descriptive message (see ["Contributing: Commit Messages"](code#commit-messages)).

To squash four commits into one, do the following:

	$ git rebase -i upstream/master

In the text editor that comes up, replace the words "pick" with "squash" or just "s" next to the commits you want to squash into the commit before it.
Save and close the editor, and git will combine the "squash"'ed commits with the one before it.
Git will then give you the opportunity to change your commit message to something like, `BUG DataObject::get_one() parameter order (fixes #100)`.

If you want to discard the commit messages from the commits you're squashing and just use the message from your "pick" commit(s) you can use "fixup" or "f" instead of "squash" to bypass the message editing and make the process a bit quicker.

Important: If you've already pushed commits to GitHub, and then squash them locally, you will have to force-push to your GitHub again.  Add the `-f` argument to your git push command:

	$ git push -f origin 100-dataobject-get-one

Helpful hint: You can always edit your last commit message by using:

	$ git commit --amend

## Working with client-side dependencies

From time to time client-side dependencies like jQuery need to be upgraded, added, or removed. We have some tools for dealing with that. Note this only applies to core SilverStripe dependencies, you're free to to manage dependencies in your project codebase however you like.

### Node.js

The [Node.js](https://nodejs.org) JavaScript runtime is the foundation of our client-side build tool chain. If you want to do things like upgrade, add, or remove libraries, you'll need Node installed on your dev environment. Our build tooling supports the v4.2.x (LTS) version of Node.

### npm

[npm](https://www.npmjs.com/) is the package manager we use for JavaScript libraries. It comes bundled with Node.js so should already have it installed if you have Node.

The configuration for an npm package goes in `package.json`. You'll see one in the root directory of `framework`. As well as being used for defining dependencies and basic package information, the `package.json` file has some handy scripts.

```
$ npm run thirdparty
```

This script copies JavaScript files from the `node_modules` directory into the `thirdparty` directory. The `node_modules` directory is not part of source control, so you'll need to run this command if you have upgraded, or added a module.

```
$ npm run sanity
```

This script makes sure files in `thirdparty` match files copied from `node_modules`. You should never commit custom changes to a library file. This script will catch them if you do :smile:

Of course to run these scripts, you'll need to get the dependencies, so run a `npm install` from the root directory to get started.

### Gulp

[Gulp](http://gulpjs.com/) is the build system which gets invoked when you run npm scripts in SilverStripe. All npm scripts have a corresponding Gulp task which you can find in `gulpfile.js`.

## Working with core JavaScript

Core JavaScript is managed by the same build tool chain as [client-side dependencies](#working-with-client-side-dependencies). Source file are located `javascript/src` directories, these are the files you should edit. Source files are input for the build tooling, which outputs distributions files, located in `javascript/dist` directories.

There are two parts of the build tooling which are important to understand when working with core JavaScript files.

### Babel

[Babel](https://babeljs.io/) is a JavaScript compiler. It takes JavaScript files as input, performs some transformations, and outputs JavaScript files. In SilverStripe we use Babel to transform our JavaScript in two ways.

#### Transforming ES6

ECMAScript 6 (ES6) is the newest version of the ECMAScript standard. It has some great new features, but the browser support is still patchy, so we use Babel to transform ES6 source files back to ES5 files for distribution.

To see some of the new features check out [https://github.com/lukehoban/es6features](https://github.com/lukehoban/es6features)

#### Transforming to UMD

[Universal Module Definition](https://github.com/umdjs/umd) (UMD) is a pattern for writing JavaScript modules. The advantage of UMD is modules can be 'required' by module loaders (AMD and ES6 / CommonJS) and can also be loaded via `<script>` tags. Here's a simple example.

```js
(function (global, factory) {
    if (typeof define === 'function' && define.amd) {
        // AMD
        define(['jQuery'], factory);
    } else if (typeof exports === 'object') {
        // CommonJS
        module.exports = factory(require('jQuery'));
    } else {
        // Default browser with no bundling (global is window)
        global.MyModule = factory(global.jQuery);
    }
}(this, function (jQuery) {
    // Module code here
}));
```

The UMD wrapper is generated by Babel so you'll never have to write it manually, all you have to do is `npm run umd`.

### Browserify bundles

One of the great new features of ES6 is [support for native modules](https://github.com/lukehoban/es6features#modules). In order to support modules, SilverStripe uses [Browserify](https://github.com/substack/node-browserify) to bundle modules for distribution.

Browserify takes in entry file, creates an abstract syntax tree (AST) by recursively looking up all the `require` statements it finds, and outputs a bundled JavaScript file which can be executed in a browser.

In addition to being a concatinated JavaScript file, Browserify bundles contain a lightweight `require()` implementation, and an API wrapper which allow modules to require each other. In most cases modules will require other modules from the same bundle. But it's also possible for bundled modules to require modules from other, external bundles.

In this example the `BetterField` module requires `jQuery` from another bundle.

__gulpfile.js__

```js
gulp.task('bundle-a', function () {
	return browserify()
		.transform(babelify.configure({
			presets: ['es2015'] // Transform ES6 to ES5.
		}))
		.require('jQuery', { expose: 'jQuery' }) // Make jQuery available to other bundles.
		.bundle()
		.pipe(source('bundle-a.js'))
		.pipe(gulp.dest('./dist'));
});
```

This generates a bundle which includes jQuery and makes it available to other bundles.

__better-field.js__

```js
import $ from 'jQuery';

$('.better-field').fadeIn();
```

__gulpfile.js__

```js

...

gulp.task('bundle-better-field', function () {
	return browserify('./src/better-field.js')
		.transform(babelify.configure({
			presets: ['es2015'] // Transform ES6 to ES5.
		}))
		.external('jQuery') // Get jQuery from another bundle.
		.bundle()
		.pipe(source('bundle-b.js'))
		.pipe(gulp.dest('./dist'));
});
```

When Browserify bundles `./src/better-field.js` (the entry file) it will ignore all require statement that refer to 'jQuery' as assume it will be available via another bundle at runtime.

To transform and bundle core JavaScript `npm run build`. This will also watch for changes to source files and re-build automatically.

For debugging you can generate a 'development' bundle which includes source maps with `npm run build --development`. Note this is for local development only and shouldn't be pushed up with a pull request.

## Some gotchas

Be careful not to commit any of your configuration files, logs, or throwaway test files to your GitHub repo. These files can contain information you wouldn't want publicly viewable and they will make it impossible to merge your contributions into the main development trunk.

Most of these special files are listed in the `.gitignore` file and won't be included in any commit, but you should carefully review the files you have modified and added before staging them and committing them to your repo. The git status command will display detailed information about any new files, modifications and staged.

	$ git status 

One thing you do not want to do is to issue a git commit with the -a option. This automatically stages and commits every modified file that's not expressly defined in .gitignore, including your crawler logs.

	$ git commit -a 

Sometimes, you might correct an issue which was reported in a different repo. In these cases, don't simply refer to the issue number as GitHub will infer that as correcting an issue in the current repo. In these cases, use the full GitHub path to reference the issue.

	$ git commit -m 'Issue silverstripe/silverstripe-cms#100: Some kind of descriptive message'

Sometimes, you might correct an issue which was reported in a different repo. In these cases, don't simply refer to the issue number as GitHub will infer that as correcting an issue in the current repo. See [Commit Messages](code#commit-messages) above for the correct way to reference these issues.

## What is git rebase?

Using `git rebase` helps create clean commit trees and makes keeping your code up-to-date with the current state of the upstream master easy. Here's how it works.

Let's say you're working on Issue #212 a new plugin in your own branch and you start with something like this:

	      1---2---3 #212-my-new-plugin
	     /
	A---B #master

You keep coding for a few days and then pull the latest upstream stuff and you end up like this:

	      1---2---3 #212-my-new-plugin
	     /
	A---B--C--D--E--F #master

So all these new things (C,D,..F) have happened since you started. Normally you would just keep going (let's say you're not finished with the plugin yet) and then deal with a merge later on, which becomes a commit, which get moved upstream and ends up grafted on the tree forever.

A cleaner way to do this is to use rebase to essentially rewrite your commits as if you had started at point F instead of point B. So just do:

git rebase master 212-my-new-plugin

git will rewrite your commits like this:

	                  1---2---3 #212-my-new-plugin
	                 /
	A---B--C--D--E--F #master

It's as if you had just started your branch. One immediate advantage you get is that you can test your branch now to see if C, D, E, or F had any impact on your code (you don't need to wait until you're finished with your plugin and merge to find this out). And, since you can keep doing this over and over again as you develop your plugin, at the end your merge will just be a fast-forward (in other words no merge at all).

So when you're ready to send the new plugin upstream, you do one last rebase, test, and then merge (which is really no merge at all) and send out your pull request. Then in most cases, we have a simple fast-forward on our end (or at worst a very small rebase or merge) and over time that adds up to a simpler tree.

More info on the ["Rebasing" chapter on git-scm.com](http://git-scm.com/book/ch3-6.html) and the [git rebase man page](http://www.kernel.org/pub/software/scm/git/docs/git-rebase.html).

## License

Portions of this guide have been adapted from the ["Thinkup" developer guide](https://github.com/ginatrapani/ThinkUp/wiki/Developer-Guide%3A-Get-the-Source-Code-from-GitHub-and-Keep-It-Updated),
with friendly permission from Gina Trapani/[thinkupapp.com](http://thinkupapp.com).