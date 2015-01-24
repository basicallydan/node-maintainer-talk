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

<!-- Makefile to build stuff -->
