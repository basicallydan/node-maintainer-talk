# I'm a Node Module Maintainer (And So Can You!)

I'm going to share with you some of the lessons I've picked up as a developer and maintainer of two or three NodeJS modules available on NPM, and from watching what others do with *their* projects. Naturally, this is just going to be the way that *I* do things. Others may have completely different ways, some of which I might find a lot better. But since this is me sharing my knowledge, please don't take it to mean that I believe it is necessarily the best way. I'm going to instruct you on how *I* would develop, organise and maintain a NodeJS module, and you can decide for yourself if it's compatible with your way of working.

## Agenda

1. I've written my Node module. Now what?
2. Inclusive code - target everyone
3. The art of a good README
4. The importance of dependency management
5. Pre-publish checklist - `npm test`, `npm shrinkwrap`, `npm version` and `npm publish`
6. Interacting with your community

### I've written my Node module. Now what?

Congratulations, you've finished a fine piece of work. Your module, `lnugTalkGenerator`, is going to make it easy for potential LNUG speakers to create new talks.

But they can't benefit from your excellent work unless they know:

- ✓ That is exists
- ✓ How to use it
- ✓ That it's *legit*
- ✓ Whether it's going to be maintained and kept up-to-date
- ✓ Whether it's reliable enough to use in production

The first step to take is to make sure you're exposing your interface for generating LNUG talks in a logical way. LNUG Talk Geenrator is suitable both for use as a global module, on the command line, and in other peoples' code, through JavaScript.

So it needs to have both a command-line interface (CLI) and a JavaScript API.

It's time to get organised. Put all the meaty bits of code into a `lib` folder in your project, and make a new file in the root of the project called `index.js`.

While the `index.js` file is crucially important to getting your command-line users their LNUG talks as quickly as possible, we need to sort out the JavaScript API first.

### Target Everyone

Examine your dependencies. Are any of them not going to be suitable for use in a browser? If they're all compatible with a JavaScript browser runtime, there's no reason why your targeted set of users shouldn't include front-end developers.

By using a tool like Browserify you can compile your code into a single JavaScript file to be used in front-end projects. Stick this in a `/bin` or `/dist` folder, and don't forget to ugilify it too!

```
browserify index.js > lnugTalkGenerator.js
uglify lnugTalkGenerator.js > lnugTalkGenerator.min.js
```

### The Art of a good README

They're called READMEs for a reason: someone looking at your project for the first time should be given all the information they need to get started with it.

That doesn't mean it needs comprehensive documentation, but for simple modules that don't do many things, you might as well put it all in there.

It doesn't stop there though: Use examples!! This is a very important rule. You need to think of some common use cases for your code and show people how your module can solve their problem in that use case.

As well as that, tell people what the change history is, how to test your module, how to compile it, and, importantly, how to contribute to it.

There are many ways to specify this information. For how to install you could use an `INSTALL.md` file, and for how to contribute you could use `CONTRIBUTING.md`. I would recommend this for larger, more mature projects, but when you're just starting out a `README` on its own is good.

You should **definitely** use Markdown rather than plain text as it's much easier to read on GitHub or npm.org (which is where most people will be reading it). It's easy to get started with Markdown - simply using headers (e.g. `###` for H3) is enough to make a big difference.

<!-- MORE HERE -->

### The Importance of Good Dependency Management

We're all familiar with `npm install` and `npm install --save`, but here are some other commands which are helpful for making sure your dependencies are properly managed.

#### `npm shrinkwrap`

What does it do? It will create a file called `npm-shrinkwrap.json`, listing all the dependencies of your project and the dependencies of *those* dependencies, and so on, but at a specific version - the version you currently have in your project.

Why is this important? It's important because not everybody respects the rules of semantic versioning, and because even when people *do* respect the rules of semantic versioning, problems can still arise between minor "non-breaking" versions.

This means that whenever anybody installs your module or clones your project and runs `npm install`, the dependency versions they get will be exactly the same as yours.

You can also specify to shrinkwrap the `devDependencies`, which will be useful for people cloning your project from its source. Do that using `npm shrinkwrap --dev`. I would recommend this most of the time.

#### `npm prune`

This is pretty useful if you're using `npm shrinkwrap`, because it will remove any installed modules which are not added as dependencies, perhaps because you were trying them out temporarily, or something. If you have un-saved dependencies, and you try to run `npm shrinkwrap`, you'll get an `extraneous` error. This is when you run `npm prune`, and start again.

#### `npm version`

This is pretty cool: not only will `npm version` update the version code in `package.json`, it'll also create a new git commit for the version, and a new git tag at that commit that looks like `v1.3.2`, a common convention for version tags on GitHub.

`npm version` takes an option to specify what type of a version bump this is: Major, meaning a change which alters existing APIs in some way; Minor, meaning an addition or change which does not modify existing APIs; and patch, usually meaning a bug fix, or some refactoring which does not affect behaviour. For example, if you've added some new config options you might use `npm version minor` - but if you change the `generateLNUGTalk` function to be called `writeBrilliantLNUGTalk`, then it's a major change because it changes an existing API.

Since `npm version` creates a git commit, then you can specify a commit message. Just like with `git commit`, you can do this with the `-m` flag.

### A pre-publish checklist

So, let's go through a quick checklist of what to do when you've got a new version of your module that you'd like to publish.

* 1: Figure out what the next version is, and remember that. If you're currently on 0.23.3 and it's a bugfix/patch, it'll be 0.23.4 next.
* 2: Update the changelog either in `README.md` or `CHANGELOG.md`. Add an entry for the day you're publishing, saying what the change was.
* 3: Make sure your dependencies are locked down. Run `npm prune` then `npm shrinkwrap --dev`.
* 4: If you're targeting the front-end, compile your code using `browserify`
* 5: Commit these changes, you don't need to mention the version code yet...
* 6: Run `npm version` with `major`, `minor`, or `patch` and a message, using `-m` - this will be your commit message.
* 7: Do a quick `git push`
* 8: You're done! `npm publish` will push changes to npm. Well done!

### Interacting with your community

This, again, is some general advice to do with open-source. Once you start open-sourcing your work and people start opening issues and pull requests, you've created a small community. Remember that in this community are real people with real feelings, who put real time into making your project a little bit better.

With that in mind, I think it is of huge importance to publicly recognise contributions. I normally make either a `contributors.md` file or put a list of contributors directly in the `README` with links to GitHub profiles so that they get a little bit of exposure for their projects when people see their names on yours. Also, whenever a new version is cut, if someone's hard work contributed to the completion of that version, I like to recognise them in the Changelog too.

I guarantee you that if you do this, not only will you encourage more contributors, you'll get a warm, fuzzy feeling inside, and so will your new open-source-co-workers.

People also need to be responded to as quickly as possible. If someone opens an issue or a pull request, you shouldn't put it off for weeks. I think it's safe to say that because you're doing this work for free, nobody expects you to treat it like a full-time job. But if you get a second simply to tell someone that their issue or PR has been seen, it's a nice thing to do. Nobody likes to be ignored! And if someone's PR isn't going to be merged, you should be honest: tell them so, and why it isn't going to happen. Perhaps they can fix a couple of things in their fork and then try again.
