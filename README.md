# Read and transfer device accelerometer data - Version 2

The application was created by  
`npx create-next-app my-project`

`npm install -D tailwindcss postcss autoprefixer`

`npx tailwindcss init -p`

## Docker build Dockerfile taken from: https://nextjs.org/docs/deployment

Build image:
`docker build . -t sensor-app`

Run image:
`docker rm sensor-app`
`docker run -p 3000:3000 --name sensor-app sensor-app`

`docker tag sensor-app:latest de.icr.io/fh-bgld/sensor-app:latest` 
`docker push de.icr.io/fh-bgld/sensor-app:latest`

`kubectl config set-context --current --namespace=sensor-app`
`kubectl rollout restart deployment sensor-app-deployment`

`kubectl delete deployment sensor-app-deployment`
`kubectl apply -f ./kubernetes/2.1_sensor_deployment.yml `

## IBM Cloud Foundry Deployment

Buildpacks:
`ibmcloud cf buildpacks`

Deployment:
`ibmcloud cf push -f manifest.yml`



# Node Red Scoring Flow
In order to send data from a browser to a Node Red http input node, Node Red has to be configured to reply to CORS requests from the browser:

1. Go to Node Red Toolchain (IBM Cloud -> Resource List -> Developer Tools -> NODERED....)
2. Open GIT Repo for Node Red
3. Edit bluemix-settings.js
4. after `functionGlobalContext: { }, ` add  
  ```
    // The following property can be used to configure cross-origin resource sharing
    // in the HTTP nodes.
    // See https://github.com/troygoode/node-cors#configuration-options for
    // details on its contents. The following is a basic permissive set of options:
    httpNodeCors: {
      origin: "*",
     methods: "GET,PUT,POST,DELETE"
    },
  ```

# IBM Cloud Toolchain Deployment

https://www.ibm.com/cloud/architecture/toolchains/develop-kubernetes-app-toolchain
https://www.ibm.com/garage/method/practices/deliver/tool_delivery_pipeline
https://www.ibm.com/cloud/architecture/tutorials/use-develop-kubernetes-app-toolchain?task=2

https://cloud.ibm.com/docs/ContinuousDelivery?topic=ContinuousDelivery-deliverypipeline_about#deploy_stage

??? https://github.com/IBM/deploy-react-kubernetes/blob/master/README.md
??? https://developer.ibm.com/tutorials/custom-toolchain-with-devops/
??? https://thomas-suedbroecker.gitbook.io/toolchain-one-microservice/deploy-the-authors-microservice-to-ibm-cloud/lab6
??? https://ibm.github.io/get-your-java-microservice-up-and-running/exercise-03/
??? https://github.com/open-toolchain/sdk/wiki/Creating-Custom-Toolchain-Templates



kubernetes deploayment. https://cloud.ibm.com/devops/setup/deploy?repository=https%3A%2F%2Feu-de.git.cloud.ibm.com%2Fopen-toolchain%2Fsecure-kube-toolchain&topic=ContinuousDelivery-getting-started&env_id=ibm%3Ayp%3Aeu-de&source_provider=githubconsolidated

1) Create a new empty toolchain
2) Add the GitHub tool, configure with github credentials and project URL
3) Add a Delivery Pipeline tool
    - add a build stage, builder type npm  
    add at the end of the build script add:  
    `npm install`  
    `npm run build`  
    - add a deploy stage, deployer type Cloud Foundry  
    **NOTE:** change the `-name: XXX-iot` in the `manifest.yml` file to your IOT organization name -> this will be the first part of the application URL!!!
    replace the deploy script with:  
    `#!/bin/bash`  
    `cp -v Staticfile build &&`  
    `cp -v manifest.yml build &&`  
    `cd build &&`  
    `cf push -f manifest.yml`  

# IOT Paltform API

https://cloud.ibm.com/docs/IoT?topic=IoT-api_overview
https://docs.internetofthings.ibmcloud.com/apis/swagger/index.html
https://docs.internetofthings.ibmcloud.com/apis/swagger/v0002/http-messaging.html#/Devices%20and%20gateways/post_device_types__typeId__devices__deviceId__events__eventName_

# kubernetes
https://stackoverflow.com/questions/40366192/kubernetes-how-to-make-deployment-to-update-image
kubectl rollout restart deployment sensor-app-deployment

# Kalman Filter
https://stackoverflow.com/questions/47210512/using-pykalman-on-raw-acceleration-data-to-calculate-position
https://ltroj.medium.com/low-cost-sensor-data-collection-in-the-field-using-your-smartphone-and-a-little-python-d7a7a9ea7b91

https://towardsdatascience.com/sensor-fusion-part-1-kalman-filter-basics-4692a653a74c
https://towardsdatascience.com/sensor-fusion-part-2-kalman-filter-code-78b82c63dcd


https://machinelearningspace.com/2d-object-tracking-using-kalman-filter/
https://medium.com/analytics-vidhya/exploring-data-acquisition-and-trajectory-tracking-with-android-devices-and-python-9fdef38f25ee


# Getting Started with Create React App

This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).

## Available Scripts

In the project directory, you can run:

### `npm start`

Runs the app in the development mode.\
Open [http://localhost:3000](http://localhost:3000) to view it in the browser.

The page will reload if you make edits.\
You will also see any lint errors in the console.

### `npm test`

Launches the test runner in the interactive watch mode.\
See the section about [running tests](https://facebook.github.io/create-react-app/docs/running-tests) for more information.

### `npm run build`

Builds the app for production to the `build` folder.\
It correctly bundles React in production mode and optimizes the build for the best performance.

The build is minified and the filenames include the hashes.\
Your app is ready to be deployed!

See the section about [deployment](https://facebook.github.io/create-react-app/docs/deployment) for more information.

### `npm run eject`

**Note: this is a one-way operation. Once you `eject`, you can’t go back!**

If you aren’t satisfied with the build tool and configuration choices, you can `eject` at any time. This command will remove the single build dependency from your project.

Instead, it will copy all the configuration files and the transitive dependencies (webpack, Babel, ESLint, etc) right into your project so you have full control over them. All of the commands except `eject` will still work, but they will point to the copied scripts so you can tweak them. At this point you’re on your own.

You don’t have to ever use `eject`. The curated feature set is suitable for small and middle deployments, and you shouldn’t feel obligated to use this feature. However we understand that this tool wouldn’t be useful if you couldn’t customize it when you are ready for it.

## Learn More

You can learn more in the [Create React App documentation](https://facebook.github.io/create-react-app/docs/getting-started).

To learn React, check out the [React documentation](https://reactjs.org/).

### Code Splitting

This section has moved here: [https://facebook.github.io/create-react-app/docs/code-splitting](https://facebook.github.io/create-react-app/docs/code-splitting)

### Analyzing the Bundle Size

This section has moved here: [https://facebook.github.io/create-react-app/docs/analyzing-the-bundle-size](https://facebook.github.io/create-react-app/docs/analyzing-the-bundle-size)

### Making a Progressive Web App

This section has moved here: [https://facebook.github.io/create-react-app/docs/making-a-progressive-web-app](https://facebook.github.io/create-react-app/docs/making-a-progressive-web-app)

### Advanced Configuration

This section has moved here: [https://facebook.github.io/create-react-app/docs/advanced-configuration](https://facebook.github.io/create-react-app/docs/advanced-configuration)

### Deployment

This section has moved here: [https://facebook.github.io/create-react-app/docs/deployment](https://facebook.github.io/create-react-app/docs/deployment)

### `npm run build` fails to minify

This section has moved here: [https://facebook.github.io/create-react-app/docs/troubleshooting#npm-run-build-fails-to-minify](https://facebook.github.io/create-react-app/docs/troubleshooting#npm-run-build-fails-to-minify)
