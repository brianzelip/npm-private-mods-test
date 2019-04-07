# npm-private-mods-test

## About

Testing .npmrc and environment variable configurations in order to install private repos via npm.

I have been unsuccessful getting private npm modules to install while keeping the secret key out of a git repository. The only solution I was able to achieve before this work was hardcoding the secret key into a .npmrc file.

I posted at least three calls for help on this issue:

1. https://stackoverflow.com/q/55514076/2145103
2. https://stackoverflow.com/q/55543361/2145103
3. https://npm.community/t/npmrc-does-not-recognize-environment-variables

## Solution

The solution to this issue is to create an environment variable via my local shell environment using .zshrc.

While I often read mentions of this method of creating env vars, I was hesitant to do so because my intuition is to _keep all project-related metadata inside the project_.

It looked to me like enough documentation hinted at the fact that npm reads from project .env files. It turns out npm does not read from project .env files, but it does recogize shell-based environment variables.

See [this answer from the npm forum](https://npm.community/t/npmrc-does-not-recognize-environment-variables/6666/2?u=brianzelip) that nudged me in the direction of success.

My personal approach to creatng persistant shell-based variables looks like this:

1. Create `~/.oh-my-zsh/custom/env.zsh`

```
export FONT_AWESOME_PRO_TOKEN="ABC123"
```

2. Import env.zsh in .zshrc

```
source $ZSH/custom/env.zsh
```

3. Call variable in .npmrc

```
@fortawesome:registry=https://npm.fontawesome.com/
//npm.fontawesome.com/:_authToken=${FONT_AWESOME_PRO_TOKEN}
```

ps - the (non osx native) man pages for [`export`](https://ss64.com/bash/export.html) and [`source`](https://ss64.com/bash/source.html) are interesting reads.
