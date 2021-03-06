# node-red-test-support #

Every piece of software should be tested - preferrably automatically. This repository contains some nodes and flows to support automated tests with Node-RED.

> Nota bene: the "system-under-test" does not necessarily have to be a Node-RED flow, it may well be an external system which is just to be tested by means of Node-RED

> Just a small note: if you like this work and plan to use it, consider "starring" this repository (you will find the "Star" button on the top right of this page), so that I know which of my repositories to take most care of.

## Prerequisites ##

The example shown in the repository requires the following Node-RED extension

* [node-red-contrib-reusable-flows](https://github.com/rozek/node-red-contrib-reusable-flows)<br>"Reusable Flows" allow multiply needed flows to be defined once and then invoked from multiple places

## Automated Tests with Node-RED ##

In a typical "Continuous-Development" setup there are several stages where parts of the created system (or the system in its entirety) have to pass numerous tests so that the development process can continue.

Usual requirements for a test environment are as follows: individual tests should be able to be executed in random order and, thus, run independent of each other (albeit not necessarily simultaneously). Single failing tests should not affect the execution of other tests - in the end, if any test failed, there should be some kind of failure report and, after fixing the underlying problem, a possibility to specifically run the previously failing test only.

The following flow tries to provide this behaviour: just copy the [included example](test-support.json) into your clipboard and import it into your Node-RED instance.

![](test-support.png)

Clicking on "run Tests" runs all tests in random order and "Status" will finally inform about overall success or failure. Individual "report" nodes indicate which tests failed. By clicking on the connected "Show" node, the failing msg object will be shown on Node-RED's debug console. Clicking on the connected "test" node at the beginning of a test flow specifically runs this test only.

Test execution is controlled by a central "dispatch" node. By setting the number of its outputs such a node can handle "n-1" tests (if "n" is the number of configured outputs). It does so by sending individual (initially empty) `msg` objects to any test in random order. Tests should be wired to outputs #2...#n, the topmost output (#1) will emit a message as soon as all tests have been run.

Large numbers of tests may be structured into "groups" with each group being controlled by its own "dispatch" node. These groups may either be "cascaded" (by wiring output #1 of a previous group to the input of a following one) or by wiring a test output (#2...#n) of a higher-level "dispatch" node to the input of a subordinated "dispatch" node (don't forget to connect the subordinate output #1 to the higher-level input in order to close the loop).

The tests themselves have been implemented as "Components". They may contain of as many nodes as needed as long as any exceptions in these nodes are caught and passed to the associated "component return" node.

Within the test functions it is completely up to the programmer to decide which test framework to use - if any. The only assumption made in this flow is that test failures should throw exceptions.

## Templates ##

Since setting up test flows is a bit cumbersome, here are a few templates - import the one that suits you most and delete any tests you don't need:

* [template-for-10-tests.json](https://raw.githubusercontent.com/rozek/node-red-test-support/main/template-for-10-tests.json) - a template for up to 10 tests

## Examples ##

Some examples for the approach shown above can be found in the author's [node-red-authorization-examples](https://github.com/rozek/node-red-authorization-examples)

## License ##

[MIT License](LICENSE.md)
