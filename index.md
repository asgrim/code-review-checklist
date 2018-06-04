## Asgrim's Code Review Checklist

### IDE tools

 * Use [Php Inspections (EA Extended)](https://plugins.jetbrains.com/idea/plugin/7622-php-inspections-ea-extended-)

### Code Style

Note: much of this should be automated - see [PHP-CS-Fixer](https://github.com/FriendsOfPHP/PHP-CS-Fixer) and [phpcs](https://github.com/squizlabs/PHP_CodeSniffer).

 * [PSR-2](https://github.com/php-fig/fig-standards/blob/master/accepted/PSR-2-coding-style-guide.md)
 * Always use `declare(strict_types=1);`
 * Remove unused `use` imports
 
### Composer.json

 * `composer require --dev roave/security-advisories dev-master`
 * Lock down PHP versions propery, e.g. `~7.0.0||~7.1.0` or `7.0.*||7.1.*` - php is NOT [semver](http://semver.org/)
 * Avoid `git`/`vcs`/etc. repo types (they're slow when using `composer update`), use [Private Packagist](https://packagist.com/) instead
 * Try to keep the "possible versions" small to resolve dependency versions quicker (e.g. if latest version of a package is `2.6.3`, use `^2.6` rather than `^2.0`)

### Class/method/function structure

 * Service classes must have `interface`s
 * Do classes use `final` [when?](http://ocramius.github.io/blog/when-to-declare-classes-final/)
 * Type declarations for all methods (including ` : void` and nullable, e.g. ` : ?string` in PHP 7.1)
 * Wrap ID values in value objects with private constructor and static initialiser (e.g. `fromString`), and implement `__toString` etc.
 * Use [beberlei/assert](https://github.com/beberlei/assert) to assert incoming arguments. Use VOs to avoid repeated validation.
 * Avoid mixed return types (`@return ObjectA|ObjectB`)
 * Don't declare variables unnecessarily (i.e. not need to declare a variable if it's only used once)

### Unit tests

 * Full coverage included with new changes
 * Fixture values are somewhat randomised, e.g.:
   * `$customerFullName = uniqid('customerFullName', true);`
   * `$uuidId = Uuid::uuid4();`
   * `$integerId = random_int(100,999);`
 * PHPUnit static methods should be called static:
   * Bad: `$this->assertSame($expectation, $actual);`
   * Good: `self::assertSame($expectation, $actual);`
 * Make sure that `$this->expectedException()` is near the method call that should raise the exception
 
### Docblocks, comments, annotations
 
 * Factories should have `@codeCoverageIgnore`
 * Tests should have `@covers \Fully\Qualified\Class\Name` annotation at class-level
 * Docblocks should exist where strict type declarations cannot be used, or intent must be described
   * Note: if intent must be described, maybe refactor?
 * Single line comments `// Should be like this` - space after `//`
 * Documentation in `/docs` committed along with the project
 
### View scripts / templates

 * Is output escaped appropriately (e.g. for Zend\View, `escapeHtml`, `escapeHtmlAttr` etc.)

### Behat tests

 * Do the scenario names make sense?
 * Simplify and abstract away complex UI interactions (e.g. caused by overuse of JS)
 
### Other tips

 * Does the code make sense?
 * Does the change solve the bug / conform to the given spec?
 * Automate running of all tests when PR created/updated (Travis, Jenkins, Gitlab etc.)
 * Automate generation of coverage - ideally report coverage back to PR
 * If not automatable, tests must be run manually - make sure it works
 * Prove functionality with E2E tests (e.g. Behat), at least with basic "happy path" scenarios
 * Naming should be explicit, but succinct
 * Typos, spelling, grammar
 * Look for general architecture issues - how does this change affect other parts of the application?
 * Too complex? Too simple?
