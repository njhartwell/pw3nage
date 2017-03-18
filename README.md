# pw3nage
If you get pw3ned, might want to fix your shell

This is a rather silly POC of a vulnerability in custom shell prompt scripts that I suspect is rather widespread. I noticed
when working on a branch that included (for the sake of cuteness) a `$` that my prompt that usually includes the branch
name had a bunch of gibberish. I suspected the zsh pluging I was using did not properly escape shell metacharacters, so
I tried a few more things and landed on this.

How it works:
1. This repo has an unusually-named default branch of `$(./pw3n)`
2. The repo contains a script at the path referenced in the branch name
3. When you cd to this repo, if your shell prompt tries to display your branch name and does't correctly escape $(..) expressions, it will execute `./pw3n`

Fixes:
 - only show whitelisted characters `branch=${BRANCH//[^a-z0-9\/]/-}`
 - construct PS1 to reference a variable that holds
   the branch name [https://github.com/git/git/commit/8976500cbbb13270398d3b3e07a17b8cc7bff43f](official git prompt fix)
