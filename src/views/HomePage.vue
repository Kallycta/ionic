<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>Blank</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true">
      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Blank</ion-title>
        </ion-toolbar>
      </ion-header>

      <div id="container">
        <strong>Ready to create an app?</strong>
        <p>Start with Ionic <a target="_blank" rel="noopener noreferrer" href="https://ionicframework.com/docs/components">UI Components</a></p>
        <button @click="startScan">startScan</button>

        <br/>
        <button @click="openCapacitorSite">OPEN</button>
        <img src="./logo_full_new.svg" alt="logo">

      </div>
    </ion-content>
  </ion-page>
</template>

<script  setup>

import { IonContent, IonHeader, IonPage, IonTitle, IonToolbar } from '@ionic/vue';

// import { Browser } from '@capacitor/browser';
import { InAppBrowser } from '@ionic-native/in-app-browser';

const openCapacitorSite = async () => {
  await InAppBrowser.open(  'https://ionic-jade.vercel.app/home' , '_self');
};

// const openCapacitorSite = async () => {
//   await Browser.open({ url: 'https://ionic-jade.vercel.app/home' });
// };

import { App } from '@capacitor/app';
import { BarcodeScanner } from '@capacitor-community/barcode-scanner';


const startScan = async () => {
 await document.querySelector('body').classList.add('scanner-active');
document.body.style.display = 'none';


  // Check camera permission
  // This is just a simple example, check out the better checks below
  await BarcodeScanner.checkPermission({ force: true });

  // make background of WebView transparent
  // note: if you are using ionic this might not be enough, check below
  BarcodeScanner.hideBackground();

  const result = await BarcodeScanner.startScan(); // start scanning and wait for a result

  // if the result has content
  if (result.hasContent) {
    console.log(result.content); // log the raw scanned content
    document.querySelector('body').classList.remove('scanner-active');
    document.body.style.display = '';
  }

};
const stopScan =  async () => {
  await BarcodeScanner.showBackground();
  await  BarcodeScanner.stopScan();
  document.querySelector('body').classList.remove('scanner-active');
  document.body.style.display = '';
};
  App.addListener('backButton', (e) => {
    console.log(e);
    stopScan()
  })



</script>

<style >
body.scanner-active {
  --background: transparent;
  --ion-background-color: transparent;
}
#container {
  text-align: center;
  
  position: absolute;
  left: 0;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
}

#container strong {
  font-size: 20px;
  line-height: 26px;
}

#container p {
  font-size: 16px;
  line-height: 22px;
  
  color: #8c8c8c;
  
  margin: 0;
}

#container a {
  text-decoration: none;
}

</style>



