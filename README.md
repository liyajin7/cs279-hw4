## How to run
1. Make sure you have a recent version of `node` installed.
2. Download this repository and open a terminal window at the location of the repo.
3. Run `npm install`.
4. Run `npm run dev`.
5. If that doesn't work, make sure that (on Mac) you also have Homebrew installed.
6. Email liyajin@college.harvard.edu with any questions!

## Important differences
Like React, Svelte is a Javascript framework, but a huge difference between this prototype and others is that our Svelte app combines all of the code that'd usually go into separate HTML, CSS, and JavaScript files into a single .svelte file. This is because Svelte also compiles its code at build time in contrast to React, Vue, or other Javascript frameworks: this makes Svelte apps incredibly reactive and fast.

Unlike all but one of our other to-do apps, this version includes a backend, Firebase. We've used a backend before (MongoDB), but Firebase is built to be more user-friendly for non-technical programmers. One huge advantage of Firebase is its extensively-detailed documentation: it certainly sped up the app-building process. It also made it easier to import a variety of different modules that simplified our Svelte script greatly.
