<h1 align="center" style="border-bottom: none;">onvif-rx-angular</h1>
<h3 align="center">A thin wrapper around onvif-rx to communicate with ONVIF devices and cameras in Angular applications.</h3>
<p align="center">
  <!-- <a href="https://github.com/semantic-release/semantic-release">
    <img alt="semantic-release" src="https://img.shields.io/badge/%20%20%F0%9F%93%A6%F0%9F%9A%80-semantic--release-e10079.svg">
  </a> -->
  <a href="https://www.npmjs.com/package/onvif-rx-angular">
    <img alt="npm latest version" src="https://img.shields.io/npm/v/onvif-rx-angular/latest.svg">
  </a>
</p>

### Installation

`npm i onvif-rx-angular onvif-rx stream`

```js
import { NgModule } from '@angular/core';
import { ONVIFModule } from 'onvif-rx-angular';

@NgModule({
  ...
  imports: [
    ...,
    ONVIFModule,
    ...
  ],
  ...
})
export class AppModule { }
```

You will also need to somehow expose a global object for the Buffer polyfill. You can do this by adding
`(window as any).global = window;` to your `polyfills.ts` file.


### Example Usage

```js
import { Component } from '@angular/core';
import { ONVIFService } from 'onvif-rx-angular';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.scss']
})
export class AppComponent {
  constructor(private onvif: ONVIFService) { }

  device = this.onvif.createManagedDevice({
    deviceUrl: 'http://192.168.1.142:80/onvif/device_service',
    password: 'admin',
    username: '123456'
  });

  getSomeInfo = () => 
    this.device.api.Media
    .GetServiceCapabilities()
    .toPromise()
    .then(response => {
      response.match({
        fail: console.log,
        ok: d => console.log(d.json)
      });
    })
}
```

### Important
- Your device must return CORS headers otherwise browsers will reject the responses. You can either run this in Electron or setup a proxy server that appends response headers.