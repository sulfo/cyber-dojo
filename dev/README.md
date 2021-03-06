
# Building your own cyber-dojo server from source

[Ensure docker is installed](http://blog.cyber-dojo.org/2017/09/running-your-own-cyber-dojo-server.html).
If you are on a Windows/Mac note that these instructions specify to
install DockerToolbox.

To get all the source:

```
mkdir src
cd src
git clone https://github.com/cyber-dojo/cyber-dojo.git
./cyber-dojo/dev/git-clone-all.sh
cd ..
```

This will create the following directory structure (each directory holds a git repo):

```
src/collector
src/cyber-dojo
src/commander
src/differ
src/grafana
src/nginx
src/prometheus
src/runner-stateful
src/runner-stateless
src/starter
src/storer
src/web
src/zipper
```

To build your server from these repos:
```
src/cyber-dojo/dev/build-all.sh
```

To bring up your server:
```
URL=https://raw.githubusercontent.com/cyber-dojo/start-points-languages/master/languages_list_common
src/commander/cyber-dojo start-point create common --list=${URL}
src/commander/cyber-dojo up --languages=common
```

To bring down your server:
```
src/commander/cyber-dojo down
```

- - - -

Each repo in the github cyber-dojo organization builds a single docker image.

These are the main service images:
  * [![Build Status](https://travis-ci.org/cyber-dojo/commander.svg?branch=master)](https://travis-ci.org/cyber-dojo/commander) [commander](https://github.com/cyber-dojo/commander) - receives commands from the [cyber-dojo](https://github.com/cyber-dojo/commander/blob/master/cyber-dojo) shell script
  * [![Build Status](https://travis-ci.org/cyber-dojo/nginx.svg?branch=master)](https://travis-ci.org/cyber-dojo/nginx) [nginx](https://github.com/cyber-dojo/nginx) - fancy internet-facing web-server
  * [![Build Status](https://travis-ci.org/cyber-dojo/web.svg?branch=master)](https://travis-ci.org/cyber-dojo/web) [web](https://github.com/cyber-dojo/web) - simple rails web-server
  * [![Build Status](https://travis-ci.org/cyber-dojo/runner-stateful.svg?branch=master)](https://travis-ci.org/cyber-dojo/runner-stateful) [runner-stateful](https://github.com/cyber-dojo/runner-stateful) - runs an avatar's code/tests statefully
  * [![Build Status](https://travis-ci.org/cyber-dojo/runner-stateless.svg?branch=master)](https://travis-ci.org/cyber-dojo/runner-stateless) [runner-stateless](https://github.com/cyber-dojo/runner-stateless) - runs an avatar's code/tests statelessly
  * [![Build Status](https://travis-ci.org/cyber-dojo/collector.svg?branch=master)](https://travis-ci.org/cyber-dojo/collector) [collector](https://github.com/cyber-dojo/collector) - collects old docker volumes created by stateful runner
  * [![Build Status](https://travis-ci.org/cyber-dojo/starter.svg?branch=master)](https://travis-ci.org/cyber-dojo/starter) [starter](https://github.com/cyber-dojo/starter) - serves the language+testFramework/exercise start-points
  * [![Build Status](https://travis-ci.org/cyber-dojo/storer.svg?branch=master)](https://travis-ci.org/cyber-dojo/storer) [storer](https://github.com/cyber-dojo/storer) - stores an avatar's code/test files
  * [![Build Status](https://travis-ci.org/cyber-dojo/differ.svg?branch=master)](https://travis-ci.org/cyber-dojo/differ) [differ](https://github.com/cyber-dojo/differ) - diffs an avatar's code/test files
  * [![Build Status](https://travis-ci.org/cyber-dojo/zipper.svg?branch=master)](https://travis-ci.org/cyber-dojo/zipper) [zipper](https://github.com/cyber-dojo/zipper) - creates tgz files for download


The main service repos each contain a `pipe_build_up_test.sh` script which:
- rebuilds the service's docker image
- starts a container from this server image
- shells into the server container
- runs the server tests (with coverage)
- starts a client container
- shells into the client container
- runs the client tests (with coverage)

- - - -

Image dependencies

![Image Dependency Graph](image_dependency_graph.png?raw=true "image dependency graph")

- - - -

Domain model

![Domain model](domain_model.png?raw=true "domain model")

