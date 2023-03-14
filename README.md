# Coding test for Platform Science

Platform Science Code Exercise

Description: this repository is a technical test by the Platform science recruitment team.

## Technical test instructions

The instructions given for the exercise are as follows:

Our sales team has just struck a deal with Acme Inc to become the exclusive provider for routing their product shipments via 3rd party trucking fleets. The catch is that we can only route one shipment to one driver per day.

Each day we get the list of shipment destinations that are available for us to offer to drivers in our network. Fortunately our team of highly trained data scientists have developed a mathematical model for determining which drivers are best suited to deliver each shipment.

With that hard work done, now all we have to do is implement a program that assigns each shipment destination to a given driver while 
maximizing the total suitability of all shipments to all drivers.

The top-secret algorithm is:

1. If the length of the shipment's destination street name is even, the base suitability score (SS) is the number of vowels in the driver’s name multiplied by 1.5.

2. If the length of the shipment's destination street name is odd, the base SS is the number of consonants in the driver’s name multiplied by 1.

3. If the length of the shipment's destination street name shares any common factors (besides 1) with the length of the driver’s name, the SS is increased by 50% above the base SS.

Write an application in the language of your choice that assigns shipment destinations to drivers in a way that maximizes the total SS over the set of drivers. Each driver can only have one shipment and each shipment can only be offered to one driver. Your program should run on the 
command line and take as input two newline separated files, the first containing the street addresses of the shipment destinations and the second containing the names of the drivers. The output should be the total SS and a matching between shipment destinations and drivers. You do not need to worry about malformed input, but you should certainly handle both upper and lower case names.


# Instructions on how to build/run the app

This project was made in vue so you must have the following requirements before install and run the project:

1. Make sure you have an up-to-date recommended version of Node.js installed. dowload and install node.js (https://nodejs.org/en/download/)

2. Make sure you have installed vue in your computer. For more info (https://learn.microsoft.com/en-us/windows/dev-environment/javascript/vue-on-windows)
```sh
npm install vue
```
3. Have installed vue/cli. For more info: (https://cli.vuejs.org/#getting-started)
```sh
npm install -g @vue/cli
```

## Install dependences

Inside the project folder install all the dependencies with the command
```sh
npm i
```
### External dependences

- Vuetify

- Eslint

- Sass

## Run the project

### Lints and fixes files
Formats the code
```
npm run lint
```
### Compiles and hot-reloads for development
Open (http://localhost:8080) to view it your favorite browser.
```
npm run serve
```

### Compiles and minifies for production
Builds the app for production to the build folder.
```
npm run build
```

# Important Notes

Before run the project it's necessary run eslint with the command:
```sh
npm run lint
```

