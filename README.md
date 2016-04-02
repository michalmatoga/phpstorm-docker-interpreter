# phpstorm-docker-interpreter

The idea was to force PhpStorm to use php interpreter that's sitting
in a container while still being able to use IDE's debugging features.

## Requirements
* Docker (Assuming [Docker Toolbox](https://www.docker.com/products/docker-toolbox))
* PhpStorm

## Getting started
### 1. (Mac users only) Configure your docker machine
OS X version of PhpStorm uses some files under `/private` by default which
is troublesome, as only files under `/Users` are mounted into docker-machine.

To get any further you need to either mount additional files:

**In VirtualBox:**

Right Click on Docker Machine -> Settings -> Shared Folders -> Add new...

* Folder path: `/private/var/folders`
* Folder name: `private_folders`


**In Docker Machine:**

```
sudo mkdir -p /private/var/folders 
sudo mount -t vboxsf -o defaults,uid=`id -u docker`,gid=`id -g docker` private_folders /private/var/folders
```

### 2. Configure PhpStorm interpreter

Clone the repository:
```
git clone git@github.com:michalmatoga/phpstorm-docker-interpreter.git
```
run `env/bin/php -v` from project's root directory - this will build an image for you and output PHP version.

Select `env/bin/php` as IDE's php interpreter:

Settings -> Languages & Frameworks -> PHP -> Interpreter -> Other Local

"PHP executable" should be set to absolute path to `env/bin/php`.

### 3. Configure PHPUnit to work with dockered interpreter

Install composer dependencies:

```
composer install
```

Configure PHPUnit

Settings -> Languages & Frameworks -> PHP -> PHPUnit

* PHPUnit Library -> Use custom autoloader -> [Select path to `vendor/autoload`]
* Default configuration file -> [Select path to `test/phpunit.xml`]

Right Click on `test` directory -> Run test

### 4. Run example application in the browser

Run

```
docker-compose up
```

Navigate your browser to `http://$(docker-machine ip default)`
