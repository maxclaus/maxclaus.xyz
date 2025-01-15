+++
title = "Error - npm doesn't install packages with GitHub dependencies"
+++

To set in your node project a dependency from github you just need to include the **<username>/<repository>** on package.json:

```sh
vagrant@srv:/srv/my-project$ npm install
npm http GET https://registry.npmjs.org/factory-mysql-fixtures
npm http 304 https://registry.npmjs.org/factory-mysql-fixtures
npm ERR! notarget No compatible version found: factory-mysql-fixtures@'maxcnunes/factory-mysql-fixtures'
npm ERR! notarget Valid install targets:
npm ERR! notarget ["0.0.2"]
npm ERR! notarget
npm ERR! notarget This is most likely not a problem with npm itself.
npm ERR! notarget In most cases you or one of your dependencies are requesting
npm ERR! notarget a package version that doesn't exist.
npm ERR! System Linux 3.2.0-23-generic
npm ERR! command "/opt/node/bin/node" "/opt/node/bin/npm" "install"
npm ERR! cwd /srv/my-project
npm ERR! node -v v0.10.25
npm ERR! npm -v 1.3.24
npm ERR! code ETARGET
npm ERR! Additional logging details can be found in:
npm ERR!     /srv/my-project/npm-debug.log
npm ERR! not ok code 0
```

If you are getting the error below then probably you don't have git installed in your environment. Install that and should fix this problem.

```json
{
  "devDependencies": {
    "factory-mysql-fixtures": "maxcnunes/factory-mysql-fixtures"
  }
}
```

