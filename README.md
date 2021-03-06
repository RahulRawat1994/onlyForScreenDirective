# OnlyForScreen Directive

A simple directive to show/hide content based on screen size

## Run

```bash
git clone <url>
npm install
npm start
```
Run `ng serve` or `npm start` for a dev server. Navigate to `http://localhost:4200/`. The app will automatically reload if you change any of the source files.

## Directive file
```js
import { Directive, ElementRef, HostListener, Input, OnInit  } from '@angular/core';
import { environment } from './../environments/environment';
interface IConfig {
  mobile: number;
  tablet: number;
}

let config: IConfig =  {
  mobile : environment.mobileWidth || 480,
  tablet : environment.tabletWidth || 720
}
// __config enviorment variable

@Directive({
  selector: '[onlyForScreen]'
})

export class OnlyForScreenDirective implements OnInit  {
  
  /**
   * @var screenWidth - for device width
   */
  screenWidth: number;

  constructor(private el: ElementRef) {}

  ngOnInit() { this.showByScreen(); }
  /**
   * @var onlyForScreen : get directive value
   */
  @Input('onlyForScreen') device: string;

  /**
   * Get current width of device
   * @param event event
   */
  @HostListener('window:resize') onResize(){
    this.showByScreen();
  }

  getScreenSize() {
    this.screenWidth = window.innerWidth;
  }

  /**
   * Function to show content on screen accroding to it's viewport width
   */
  showByScreen(){
    this.getScreenSize();
    switch(this.device){

      case 'mobile':
          if(this.screenWidth <= config[this.device]){
            this.el.nativeElement.style.display = 'block';
          } else {
            this.el.nativeElement.style.display = 'none';
          }
       
        break;

      case 'tablet':
          if(this.screenWidth <= config[this.device] && this.screenWidth >= config['mobile']){
            this.el.nativeElement.style.display = 'block';
          } else {
            this.el.nativeElement.style.display = 'none';
          }
        break;

      case 'desktop':
        if(this.screenWidth <= config['tablet']){
          this.el.nativeElement.style.display = 'none';
        } else {
          this.el.nativeElement.style.display = 'block';
        }
        break;

      default:
        this.el.nativeElement.style.display = 'block';
        break;
    }
  }
}

```

## Env Variable

```js
// for Dev
export const environment = {
  production: false,
  mobileWidth : 420, 
  tabletWidth : 800
};
// for production
export const environment = {
  production: true,
  mobileWidth : 420, 
  tabletWidth : 800
};

```

## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

Please make sure to update tests as appropriate.

## License
[MIT](https://choosealicense.com/licenses/mit/)