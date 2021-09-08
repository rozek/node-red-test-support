# node-red-test-support #

Every piece of software should be tested - preferrably automatically. This repository contains some nodes and flows to support automated tests with Node-RED.

> Nota bene: the "system-under-test" does not necessarily have to be a Node-RED flow, it may well be an external system which is just to be tested by means of Node-RED

## Prerequisites ##

The example shown in the repository requires the following Node-RED extension

* [node-red-contrib-components](https://github.com/ollixx/node-red-contrib-components)<br>"Components" allow multiply needed flows to be defined once and then invoked from multiple places

## Automated Tests with Node-RED ##

In a typical "Continuous-Development" setup there are several stages where parts of the developed system (or the system in its entirety) have to pass numerous tests so that the process can continue.

The individual tests should be able to be executed in random order and, thus, be independent of each other. Single failing tests should not affect execution of other tests - in the end, if any test failed, there should be some kind of failure report and, after fixing the underlying problem, a possibility to specifically run the previously failing test only.

The following flow tries to provide this behaviour: just copy [the example](test-support.json) into your clipboard and import it into your Node-RED instance.

![](test-support.png)

## License ##

[MIT License](LICENSE.md)
