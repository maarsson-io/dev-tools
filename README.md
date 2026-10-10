
# maarsson/dev-tools

Development QA toolchain bundle for my projects.

<div aria-hidden="true">

[![Latest Stable Version](https://img.shields.io/github/v/release/maarsson/dev-tools?label=Latest)](https://github.com/maarsson/dev-tools/releases)
![Minimum PHP Version](https://img.shields.io/packagist/dependency-v/maarsson/dev-tools/php.svg)
[![Tested on PHP 8,4 to 8.5](https://img.shields.io/badge/tested%20on-PHP%208.4%20|%208.5-brightgreen.svg?maxAge=2419200)][GHA-test]
[![Test](https://github.com/maarsson/dev-tools/actions/workflows/ci.yml/badge.svg?branch=master)][GHA-test]
[![License](https://img.shields.io/github/license/maarsson/dev-tools)](https://github.com/maarsson/dev-tools/blob/master/LICENSE)

[GHA-test]: https://github.com/maarsson/dev-tools/actions/workflows/ci.yml

</div>

> [!NOTE]
> See also [maarsson/coding-standard](https://github.com/maarsson-io/coding-standard).

## About

This package is a Composer metapackage that installs a curated set of development and code-quality tools together with shared coding standards.

Installing this package will pull in:

- `phpmd/phpmd` – [PHP Mess Detector (PHPMD)](https://phpmd.org/) - detect design and complexity issues
- `squizlabs/php_codesniffer` – [PHP CodeSniffer (PHPCS)](https://github.com/PHPCSStandards/PHP_CodeSniffer/) - detect coding standard violations
- `slevomat/coding-standard` – [Slevomat Coding Standard](https://github.com/slevomat/coding-standard/) - additional PHPCS checks
- `friendsofphp/php-cs-fixer` – [PHP CS Fixer](https://cs.symfony.com/) - automatically enforce modern code style
- `larastan/larastan` – [Larastan](https://github.com/larastan/larastan) - catches both obvious & tricky bugs
- `phpro/grumphp` – [GrumPHP](https://github.com/phpro/grumphp/) - run configured code-quality checks before commits
- `maarsson/coding-standard` – [Centralized standards](https://github.com/maarsson-io/coding-standard) - shared coding standards and sync tooling - follow its [project configuration instructions](https://github.com/maarsson-io/coding-standard#2-project-configuration-required) to enable automatic ruleset synchronization and Git hooks.

By following the installation steps below, the rulesets from the installed version of `maarsson/coding-standard` are automatically applied after `composer install` and `composer update` in your project. This guarantees that all projects use the exact same ruleset versions.

The `extra.frontend-tools` setting defines the npm dependencies for ESLint including Stylistic, Stylelint, TypeScript, Vue, SCSS, and Tailwind support. The coding-standard sync script adds them to the consuming project's `devDependencies` and creates the `eslint`, `eslint:fix`, `stylelint`, and `stylelint:fix` scripts.

---

## Requirements

- PHP ^8.4
- Composer
- For the frontend tools: npm and Node.js `^22.13.0 || >=24`

---

## Installation

### 1. Package installation

Install the package as a development dependency in your project:

```sh
composer config --no-plugins allow-plugins.dealerdirect/phpcodesniffer-composer-installer true
composer config --no-plugins allow-plugins.phpro/grumphp false
composer require --dev maarsson/dev-tools
```

These plugin permissions must be configured in the consuming project's `composer.json`. They are not inherited from this metapackage. The PHPCS installer registers the Slevomat standard, and the GrumPHP plugin installs Git hooks.

### 2. Project configuration (required)

To ensure the coding standards are applied automatically, you must configure Composer scripts in the target project.

Follow the [coding-standard Composer hook configuration](https://github.com/maarsson-io/coding-standard#2-project-configuration-required), including its conditional `coding-standard:sync` script. The condition skips synchronization during `--no-dev` installs and updates, when the development tools and sync executable are absent.

#### Install the frontend tools

Composer install/update synchronizes `package.json` but does not install npm packages. The sync script displays a reminder when it updates `package.json`.

After the initial sync, and whenever a toolchain update changes the npm dependencies, install them manually in the consuming project:

```sh
npm install --ignore-scripts
```

Commit the resulting `package.json` and `package-lock.json` together.

In CI, run `npm ci --ignore-scripts` after Composer installation to install from the committed lockfile.

---

## Usage

For more info please read the `maarsson/coding-standard` package's [readme](https://github.com/maarsson-io/coding-standard#readme).

---

## Design philosophy

- `maarsson/dev-tools` defines what tools are installed
- `maarsson/coding-standard` defines how rulesets are applied
- The project decides when commands run

This separation keeps behavior explicit, predictable, and Composer-idiomatic.

---

## License

[MIT](LICENSE)
