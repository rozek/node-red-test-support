# node-red-test-support #

Every piece of software should be tested - preferrably automatically. This repository contains some nodes and flows to support automated tests with Node-RED.

> Nota bene: the "system-under-test" does not necessarily have to be a Node-RED flow, it may well be an external system which is just to be tested by means of Node-RED

## Prerequisites ##

The example shown in the repository requires the following Node-RED extension

* [node-red-contrib-components](https://github.com/ollixx/node-red-contrib-components)<br>"Components" allow multiply needed flows to be defined once and then invoked from multiple places

## Automated Tests with Node-RED ##

In a typical "Continuous-Development" setup there are several stages where parts of the created system (or the system in its entirety) have to pass numerous tests so that the development process can continue.

Individual tests should be able to be executed in random order and, thus, run independent of each other (albeit not necessarily simultaneously). Single failing tests should not affect the execution of other tests - in the end, if any test failed, there should be some kind of failure report and, after fixing the underlying problem, a possibility to specifically run the previously failing test only.

The following flow tries to provide this behaviour: just copy the [included example](test-support.json) into your clipboard and import it into your Node-RED instance.

![](test-support.png)

Clicking on "run Tests" runs all tests in random order and "Status" will finally inform about overall success or failure. Individual "report" nodes indicate which tests failed. By clicking on the connected "Show" node, the failing msg object will be shown on Node-RED's debug console. Clicking on the connected "test" node at the beginning of a test flow specifically runs this test only.

Test execution is controlled by a central "dispatch" node. By setting the number of its outputs such a node can handle "n-1" tests (if "n" is the number of configured outputs). It does so by sending individual (initially empty) `msg` objects to any test in random order. Tests should be wired to outputs #2...#n, the topmost output (#1) will emit a message as soon as all tests have been run.

Large numbers of tests may be structured into "groups" with each group being controlled by its own "dispatch" node. These groups may either be "cascaded" (by wiring output #1 of a previous group to the input of a following one) or by wiring a test output (#2...#n) of a higher-level "dispatch" node to the input of a subordinated "dispatch" node (don't forget to connect the subordinate output #1 to the higher-level input in order to close the loop).

The tests themselves have been implemented as "Components". They may contain of as many nodes as needed as long as any exceptions in these nodes are caught and passed to the associated "component return" node.

Within the test functions it is completely up to the programmer to decide which test framework to use - if any. The only assumption made in this flow is that test failures should throw exceptions.

## License ##

[MIT License](LICENSE.md)
