# I'm a node module maintainer and so can you!

I'm going to share with you some of the lessons I've picked up as a developer and maintainer of two or three NodeJS modules available on NPM, and from watching what others do with *their* projects. Naturally, this is just going to be the way that *I* do things. Others may have completely different ways, some of which I might find a lot better. But since this is me sharing my knowledge, please don't take it to mean that I believe it is necessarily the best way. I'm going to instruct you on how *I* would develop, organise and maintain a NodeJS module, and you can decide for yourself if it's compatible with your way of working.

## Agenda

1. I've written my Node module. Now what?
2. Inclusive code - target everyone
3. The art of a good README
4. The importance of dependency management
5. Pre-publish checklist - `npm test`, `npm shrinkwrap`, `npm version` and `npm publish`
6. Interacting with your community

### I've written my Node module. Now what?

Congratulations, you've finished a fine piece of work. Your module, `randomCheeseGenerator`, is going to enrich the programming lives of hundreds of dairy enthusiasts.

But they can't benefit from your excellent work unless they know:

- ✓ That is exists
- ✓ How to use it
- ✓ That it's *legit*
- ✓ Whether it's going to be maintained and kept up-to-date
- ✓ Whether it's reliable enough to use in production

The first step to take is to make sure you're exposing your interface for generating random cheese in a logical way. Random Cheese Generator is suitable both for use as a global module, on the command line, and in other peoples' code, through JavaScript.

So it needs to have both a command-line interface (CLI) and a JavaScript API.

It's time to get organised. Put all the meaty bits of code into a `lib` folder in your project, and make a new file in the root of the project called `index.js`.

While the `index.js` file is crucially important to getting your command-line users their random cheeses as quickly as possible, we need to sort out the JavaScript API first.

### Target Everyone

Examine your dependencies. Are any of them not going to be suitable for use in a browser? If they're all compatible with a JavaScript browser runtime, there's no reason why your targeted set of users shouldn't include front-end developers.

By using a tool like Browserify you can compile your code into a single JavaScript file to be used in front-end projects. Stick this in a `/bin` or `/dist` folder, and don't forget to ugilify it too!

```
browserify index.js > randomCheeseGenerator.js
uglify randomCheeseGenerator.js > randomCheeseGenerator.min.js
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

<!-- MORE HERE -->

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

<!-- Make sure to reply to pull requests and issues: a reply telling them that you've seen it is better than leaving it for weeks without saying anything -->

<!-- Makefile to build stuff -->
