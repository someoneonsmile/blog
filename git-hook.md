# git hook

## install commitlint

```sh
# Install and configure if needed
npm install -D @commitlint/{cli,config-conventional}
# For Windows:
npm install -D @commitlint/config-conventional @commitlint/cli

# Configure commitlint to use conventional config
echo "module.exports = { extends: ['@commitlint/config-conventional'] };" > commitlint.config.js
```

[doc](https://commitlint.js.org/#/guides-local-setup?id=install-husky)

## install commit prompt (gitmoji)

todo!()

## install husky

- install cli

```sh
npm install -g husky
# or install as dev deepency
npm install -D husky
```

- install husky for the git repo

```sh
husky install
```

- add hook

```sh
npx husky add .husky/commit-msg 'npx --no -- commitlint --edit $(1)'
```
