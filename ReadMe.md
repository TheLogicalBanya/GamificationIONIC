# Gamification for android & ios platform

This package provide integration with gamification provided by The Logical Banya.

## Installation

You can install this package using npm:

```bash
npm i gamification-ionic
```

Install package for screen lock orientation:

```bash
npm install @capacitor/screen-orientation
```

```bash
import {ScreenOrientation} from '@capacitor/screen-orientation'

```

## Usage

1.create new page and import Gamification package & pass configuration to init method.<br>

import package

```bash
import Gamification from "gamification-ionic";
```

initialize config

```bash
const config = {
  baseUrl: "",
  clientID: "",
  key: "",
  userID: "",
  username: "",
  keyString: "",
  appId: 'app' /* pass div id in used index.html like  <body> <div id="app"></div>< /body> */
  utm_param1: 'ionic' // optional parameters 
  utm_param2 :'' // optional parameters 
  utm_param3 :'' // optional parameters 
  utm_param4 :'' // optional parameters 
  style: {height: '100vh'} // optional parameters you can pass styles here height,width,left,zIndex,top
  
};
Gamification.init(config);
```

```bash
Required parameters

baseUrl: "",
clientID: "",
key: "",
userID: "",
username: "",
keyString: "",
utm_param1: 'ionic'

Take above parameters from provider.

```

Create new method to open & close application.

```bash

const isOpen = ref(false);

const openApp = async () => {
  //this code if need  screen rotation prevents start
  try {
    await ScreenOrientation.lock({orientation: 'portrait'});
  } catch (e) {
    console.error(e.message)
  }
  //this code for screen rotation prevents end

  await Gamification.run();
  isOpen.value = true;
}

// for close gamification window
window.addEventListener('message', async (event) => {
  // Check origin if needed for security
  const {data} = event;
  if (data === 'close-app') {
    await Gamification.close();
    await ScreenOrientation.unlock();
  }
  // Handle the received data
});

```

Create method for closing application.

```bash
const closeApp =async () => {
  Gamification.close();
  await ScreenOrientation.unlock();
}
```

Here is an example of implementation.

```
<template>
  <ion-page>
    <ion-content :fullscreen="true">
      <div style="padding: 10px">
        <IonButton @click="openApp" type="button" mode="md" color="primary">
          IONIC Test
        </IonButton>
      </div>
    </ion-content>
  </ion-page>
</template>

<script setup>
import {IonButton, IonContent, IonPage} from '@ionic/vue';
import Gamification from "gamification-ionic";
import {ref} from "vue";
import {ScreenOrientation} from '@capacitor/screen-orientation';

Gamification.init({
  baseUrl: "",
  clientID: "",
  key: "",
  userID: "",
  username: "",
  keyString: "",
  appId: 'app', /* pass div id in used index.html like  <body> <div id="app"></div>< /body> */
  utm_param1: 'ionic'
  // style: {
  //   height: 'calc(100vh - 70px)',
  //   top: '70px',
  // }
});
const isOpen = ref(false);

const openApp = async () => {
//this code if need  screen rotation prevents start
  try {
    await ScreenOrientation.lock({orientation: 'portrait'});
  } catch (e) {
    console.error(e.message)
  }
//this code if need  screen rotation prevents end

  await Gamification.run();
  isOpen.value = true;

}

  // for close gamification window
window.addEventListener('message', async (event) => {
  // Check origin if needed for security
  const {data} = event;
  if (data === 'close-app') {
    await Gamification.close();
    await ScreenOrientation.unlock();
  }
  // Handle the received data
});
</script>

    
```

Also you can pass another optional parameters <br>

```bash
2.utm_param2 <br>
3.utm_param3 <br>
4.utm_param4 <br>
5.style: {
    height: '100vh',
    width:100%,
} // optional parameters you can pass to style key: height,width,left,zIndex,top<br>

```

