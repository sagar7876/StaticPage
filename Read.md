## File: README.md

**File Content:**

<h2>https://amitxshukla.github.io</h2>

<a href="https://youtu.be/IGJQ3w4LMjw" target="_blank">[Click here for Video Tutorials !]</a>

<h4> Objective: Angular Material - Static Website</h4>
In this tutorial, we will be using Angular Material to build a static website.
And in follow up tutorials, further We will enhance this webstie and implement Angular Universal and SEO for server side rendering and Search engine and social search capabilities.

I assume you already have a successful Angular Development environment already setup and 
if not, please visit 

<a href="https://github.com/AmitXShukla/Angular-Capacitor-Firebase-Setup" target="_blank">https://github.com/AmitXShukla/Angular-Capacitor-Firebase-Setup</a>

First let’s install Angular Material into our App.

<h4>Step 1: Start with a brand new Angular App.</h4>
Let's quickly confirm angular setup/installation

<span style="color:green">VSCODE > Terminal > node -v</span>  // show current node version installed

<span style="color:green">npm -v</span> // show current npm version

<span style="color:green">ng -v</span> // show current Angular-Cli version

<span style="color:green">ng new amitxshukla.github.io</span> // create a new ng app

<span style="color:green"> cd amitxshukla.github.io</span>

<span style="color:green"> ng serve</span>

<h4> Clean up tasks:</h4>
<b>update favicon</b> - 
update index.html to pick favicon from assets/icon directory and add copy new favicon.ico file to assets/icon folder

<h4>update polyfill.json</h4> - In case of using web animations or if you are planning to support older browser versions.

and in case of adding a polyfill, please make sure to npm install related package for chosen polyfill.

<h4>update gitignore file</h4>
- Please include files/folder which you do not wish to include to Git Repository like <dist> or <node_modules> folder
OR environment.ts, environment.prod.ts etc

<h4>update Angular-cli.json settings </h4>
"prefix": "app" 
get rid of "app" prefix, otherwise, this setting will prefix, all selector with this string, like FooterComponent will have selector = app-footer

<h4> Step 2: Install Angular Materials</h4>
<span style="color:green">npm install --save @angular/material @angular/cdk</span>
<span style="color:green">npm install --save @angular/animations</span>

Note: @angular/animations uses the WebAnimation API that isn't supported by all browsers yet. If you want to support Material component animations in these browsers, you'll have to include a polyfill.

<h4> Step 3: Include Angular Material Module</h4>
You have the option to either include individual Angular Material components or
Create a new custom material module and import this module
Whichever approach you use, be sure to import the Angular Material modules after Angular's BrowserModule, as the import order matters for NgModules.

create a new custom material module and include this is app.module

(These step is included below in development tasks)

<h4> Step 4. Include a theme</h4>
add this to styles.css or copy this over to assets/css directory

<span style="color:green">@import "~@angular/material/prebuilt-themes/indigo-pink.css";</span>

<h4>Step 5. Gesture support</h4>
Some components (mat-slide-toggle, mat-slider, matTooltip) rely on HammerJS for gestures

<span style="color:green">npm install --save hammerjs</span>

<h4>Step 6 (Optional): Add Material Icons</h4>
If you want to use the mat-icon component with the official Material Design Icons, load the icon font in your index.html.

<span style="color:green">link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet"</span>

For more information on using Material Icons, check out the Material Icons Guide.
Note that mat-icon supports any font or svg icons; using Material Icons is one of many options.
_________________________

<h4>Development Tasks</h4>
create routes.ts

<span style="color:green">ng g interface shared/routes</span>

<h4> replace routes.ts text with </h4>
https://github.com/AmitXShukla/Angular-Material/blob/master/src/app/shared/routes.ts

<h4> include APP_ROUTES</h4> from shared/routes.ts to app.module.ts  
<span style="color:green">(add to IMPORT section)</span>

<h4>create custom material module</h4>
<span style="color:green">ng g module shared/custom.material</span>

<h4>copy below content to custom.material.ts file</h4>

https://github.com/AmitXShukla/Angular-Material/blob/master/src/app/shared/custommaterial/custommaterial.module.ts

<h4>update app.module.ts</h4> to include ElishCustomMaterialModule in IMPORT section 
<span style="color:green">(add to IMPORT section)</span>

<h4>Now You have all Angular Material dependencies installed.</h4>

_________________________

<h4>Define Website Content and requirements</h4>
This website will need a top tool bar.

Left most icon at top tool bar will act as a home button which further toggle a left navigation.
(by default once leftnav is open, it stays open and can be closed by home icon at top left)

Left navigation Menu Items should include links to -
    Subscribe/ Contact
    About me    (Tab layout   About Me    Askus Terms)
    IT Services
    Projects
    Blogs
    Videos
    Terms

Top tool bar should also include a mat-icon and header/title text for the page being displayed in middle.
At right most side of tool bar, it should include mat-icons for email, settings and social icon for contact

<b><i> All mat-icons must have a tool tip displayed</i></b>

At the bottom of the page, a footer needs to be included.
This footer will have copyright statement and include all navigation links for SEO rendering.

_________________________

<h4>Development tasks</h4>
Now it's time to add header and footer components

create a new footer 

<span style="color:green">ng g component shared/footer</span>

update footer.html with this text
<span style="color:green">copyright 2018, amitxshukla.github.io</span>

<span style="color:green">import FOOTER Component to app.module.ts  (add to <declarations> section)</span>

update app.component.html file and include at last

<span style="color:green">< footer >< /footer ></span>

<span style="color:green"> ng serve</span>

at this point to make sure, app displays footer statement and is running fine.

<h4>add a new header</h4>

Open app.component.ts and

Include tool bar code

Include material icons

Include Left Navigation

Fix, person_add mat-icon to include links from social plugin

register a new custom svgicon

Fix Left-Hand navigation

Create one page and link to this page to router-outlet and routes.ts

<h4>Register a new custom svgicon</h4>

in case of error, include HttpClientModule


<span style="color:green">Convert all MAT-ICON for offline use</span>

## File: karma.conf.js

**File Content:**

// Karma configuration file, see link for more information
// https://karma-runner.github.io/1.0/config/configuration-file.html

module.exports = function (config) {
  config.set({
    basePath: '',
    frameworks: ['jasmine', '@angular-devkit/build-angular'],
    plugins: [
      require('karma-jasmine'),
      require('karma-chrome-launcher'),
      require('karma-jasmine-html-reporter'),
      require('karma-coverage-istanbul-reporter'),
      require('@angular-devkit/build-angular/plugins/karma')
    ],
    client: {
      clearContext: false // leave Jasmine Spec Runner output visible in browser
    },
    coverageIstanbulReporter: {
      dir: require('path').join(__dirname, './coverage/amitshukla'),
      reports: ['html', 'lcovonly', 'text-summary'],
      fixWebpackSourcePaths: true
    },
    reporters: ['progress', 'kjhtml'],
    port: 9876,
    colors: true,
    logLevel: config.LOG_INFO,
    autoWatch: true,
    browsers: ['Chrome'],
    singleRun: false,
    restartOnFileChange: true
  });
};


## File: src/app/app.component.css

**File Content:**

:host {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
    font-size: 14px;
    color: #333;
    box-sizing: border-box;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }

  h1,
  h2,
  h3,
  h4,
  h5,
  h6 {
    margin: 8px 0;
  }

  p {
    margin: 0;
  }

  .spacer {
    flex: 1;
  }

  .toolbar {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    height: 60px;
    display: flex;
    align-items: center;
    background-color: #1976d2;
    color: white;
    font-weight: 600;
  }

  .toolbar img {
    margin: 0 16px;
  }

  .toolbar #twitter-logo {
    height: 40px;
    margin: 0 16px;
  }

  .toolbar #twitter-logo:hover {
    opacity: 0.8;
  }

  .content {
    display: flex;
    margin: 82px auto 32px;
    padding: 0 16px;
    max-width: 960px;
    flex-direction: column;
    align-items: center;
  }

  svg.material-icons {
    height: 24px;
    width: auto;
  }

  svg.material-icons:not(:last-child) {
    margin-right: 8px;
  }

  .card svg.material-icons path {
    fill: #888;
  }

  .card-container {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    margin-top: 16px;
  }

  .card {
    border-radius: 4px;
    border: 1px solid #eee;
    background-color: #fafafa;
    height: 40px;
    width: 200px;
    margin: 0 8px 16px;
    padding: 16px;
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    transition: all 0.2s ease-in-out;
    line-height: 24px;
  }

  .card-container .card:not(:last-child) {
    margin-right: 0;
  }

  .card.card-small {
    height: 16px;
    width: 168px;
  }

  .card-container .card:not(.highlight-card) {
    cursor: pointer;
  }

  .card-container .card:not(.highlight-card):hover {
    transform: translateY(-3px);
    box-shadow: 0 4px 17px rgba(0, 0, 0, 0.35);
  }

  .card-container .card:not(.highlight-card):hover .material-icons path {
    fill: rgb(105, 103, 103);
  }

  .card.highlight-card {
    background-color: #1976d2;
    color: white;
    font-weight: 600;
    border: none;
    width: auto;
    min-width: 30%;
    position: relative;
  }

  .card.card.highlight-card span {
    margin-left: 60px;
  }

  svg#rocket {
    width: 80px;
    position: absolute;
    left: -10px;
    top: -24px;
  }

  svg#rocket-smoke {
    height: calc(100vh - 95px);
    position: absolute;
    top: 10px;
    right: 180px;
    z-index: -10;
  }

  a,
  a:visited,
  a:hover {
    color: #1976d2;
    text-decoration: none;
  }

  a:hover {
    color: #125699;
  }

  .terminal {
    position: relative;
    width: 80%;
    max-width: 600px;
    border-radius: 6px;
    padding-top: 45px;
    margin-top: 8px;
    overflow: hidden;
    background-color: rgb(15, 15, 16);
  }

  .terminal::before {
    content: "\2022 \2022 \2022";
    position: absolute;
    top: 0;
    left: 0;
    height: 4px;
    background: rgb(58, 58, 58);
    color: #c2c3c4;
    width: 100%;
    font-size: 2rem;
    line-height: 0;
    padding: 14px 0;
    text-indent: 4px;
  }

  .terminal pre {
    font-family: SFMono-Regular,Consolas,Liberation Mono,Menlo,monospace;
    color: white;
    padding: 0 1rem 1rem;
    margin: 0;
  }

  .circle-link {
    height: 40px;
    width: 40px;
    border-radius: 40px;
    margin: 8px;
    background-color: white;
    border: 1px solid #eeeeee;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.12), 0 1px 2px rgba(0, 0, 0, 0.24);
    transition: 1s ease-out;
  }

  .circle-link:hover {
    transform: translateY(-0.25rem);
    box-shadow: 0px 3px 15px rgba(0, 0, 0, 0.2);
  }

  footer {
    margin-top: 8px;
    display: flex;
    align-items: center;
    line-height: 20px;
  }

  footer a {
    display: flex;
    align-items: center;
  }

  .github-star-badge {
    color: #24292e;
    display: flex;
    align-items: center;
    font-size: 12px;
    padding: 3px 10px;
    border: 1px solid rgba(27,31,35,.2);
    border-radius: 3px;
    background-image: linear-gradient(-180deg,#fafbfc,#eff3f6 90%);
    margin-left: 4px;
    font-weight: 600;
    font-family: -apple-system,BlinkMacSystemFont,Segoe UI,Helvetica,Arial,sans-serif,Apple Color Emoji,Segoe UI Emoji,Segoe UI Symbol;
  }

  .github-star-badge:hover {
    background-image: linear-gradient(-180deg,#f0f3f6,#e6ebf1 90%);
    border-color: rgba(27,31,35,.35);
    background-position: -.5em;
  }

  .github-star-badge .material-icons {
    height: 16px;
    width: 16px;
    margin-right: 4px;
  }

  svg#clouds {
    position: fixed;
    bottom: -160px;
    left: -230px;
    z-index: -10;
    width: 1920px;
  }


  /* Responsive Styles */
  @media screen and (max-width: 767px) {

    .card-container > *:not(.circle-link) ,
    .terminal {
      width: 100%;
    }

    .card:not(.highlight-card) {
      height: 16px;
      margin: 8px 0;
    }

    .card.highlight-card span {
      margin-left: 72px;
    }

    svg#rocket-smoke {
      right: 120px;
      transform: rotate(-5deg);
    }
  }

  @media screen and (max-width: 575px) {
    svg#rocket-smoke {
      display: none;
      visibility: hidden;
    }
  }

## File: src/app/app.component.html

**File Content:**

<mat-card>
    <mat-card-content>
        <router-outlet></router-outlet>
    </mat-card-content>
    <mat-card-actions>
        <app-footer></app-footer>
    </mat-card-actions>
</mat-card>

## File: src/app/shared/footer.component.css

**File Content:**



## File: src/app/shared/footer.component.html

**File Content:**

<div class="noprint">
    <a title="FaceBook" href="https://www.facebook.com/elishconsulting" target="_blank" rel="noopener">
        <mat-icon matTooltip="connect with FaceBook" class="circle-link example-icon-link" svgIcon="facebook">
        </mat-icon>
    </a>
    <a title="GitHub" href="https://github.com/AmitXShukla" target="_blank" rel="noopener">
        <mat-icon matTooltip="connect with GitHub" class="circle-link example-icon-link" svgIcon="github"></mat-icon>
    </a>
    <a title="LinkedIn" href="https://www.linkedin.com/in/elishconsulting/" target="_blank" rel="noopener">
        <mat-icon matTooltip="connect with LinkedIn" class="circle-link example-icon-link" svgIcon="linkedin">
        </mat-icon>
    </a>
    <a title="Twitter" href="https://twitter.com/ashuklax" target="_blank" rel="noopener">
        <mat-icon matTooltip="connect with Twitter" class="circle-link example-icon-link" svgIcon="twitter"></mat-icon>
    </a>
    <a title="YouTube" href="https://youtube.com/amitshukla_ai" target="_blank" rel="noopener">
        <mat-icon matTooltip="connect with YouTube" class="circle-link example-icon-link" svgIcon="youtube"></mat-icon>
    </a>
    <a title="Medium Blogs" href="https://medium.com/@Amit_Shukla" target="_blank" rel="noopener">
      <mat-icon matTooltip="connect with Medium Blogs" class="circle-link example-icon-link" svgIcon="medium"></mat-icon>
  </a>
    <br />
    <!-- Footer -->
    <span class="example-icon">
        @copyright 2020 Amit Shukla https://amitxshukla.github.io
        <a href="https://elishconsulting.com" target="_blank" rel="noopener">
            <svg class="material-icons" xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24">
                <path d="M10 6L8.59 7.41 13.17 12l-4.58 4.59L10 18l6-6z" fill="#1976d2" />
                <path d="M0 0h24v24H0z" fill="none" /></svg>
        </a>
    </span>
</div>

## File: src/app/shared/header.component.css

**File Content:**

.example-icon {
    padding: 0 14px;
  }
  
  .example-spacer {
    flex: 1 1 auto;
  }
  

## File: src/app/shared/header.component.html

**File Content:**

<mat-toolbar color="primary">
    <mat-toolbar-row>
        <button mat-mini-fab color="warn" aria-label="Menu" [matMenuTriggerFor]="rootMenu">
            <mat-icon>menu</mat-icon>
          </button>
        <span class="small-spacer"></span>
        <mat-icon>{{ imageUrl }}</mat-icon>
        <span class="small-spacer"></span>
        <span matTooltip="About us">{{ pageTitle }}</span>
        <span class="example-spacer"></span>
        <img src="../assets/icons/favicon.ico" width="40" height="40" class="circle-link" [matMenuTriggerFor]="contactMenu">
      </mat-toolbar-row>  
  </mat-toolbar>
  <mat-menu #rootMenu="matMenu">
    <button mat-menu-item routerLink="/aboutus">
      <mat-icon>dashboard</mat-icon>
      <span>About us</span>
    </button>
    <mat-divider></mat-divider>
    <button mat-menu-item routerLink="/technology">
      <mat-icon>build</mat-icon>
      <span>By Technology</span>
    </button>
    <mat-divider></mat-divider>
    <button mat-menu-item routerLink="/business">
      <mat-icon>business</mat-icon>
      <span>By Business</span>
    </button>
    <mat-divider></mat-divider>
    <button mat-menu-item routerLink="/pro">
      <mat-icon>donut_small</mat-icon>
      <span>Pro</span>
    </button>
  </mat-menu>
  <mat-menu #contactMenu="matMenu">
    <a title="FaceBook" href="https://www.facebook.com/elishconsulting" target="_blank" rel="noopener">
        <mat-icon matTooltip="connect with FaceBook" class="circle-link example-icon-link" svgIcon="facebook">
        </mat-icon>
    </a>
    <a title="GitHub" href="https://github.com/AmitXShukla" target="_blank" rel="noopener">
        <mat-icon matTooltip="connect with GitHub" class="circle-link example-icon-link" svgIcon="github"></mat-icon>
    </a>
    <a title="LinkedIn" href="https://www.linkedin.com/in/elishconsulting/" target="_blank" rel="noopener">
        <mat-icon matTooltip="connect with LinkedIn" class="circle-link example-icon-link" svgIcon="linkedin">
        </mat-icon>
    </a>
    <a title="Twitter" href="https://twitter.com/ashuklax" target="_blank" rel="noopener">
        <mat-icon matTooltip="connect with Twitter" class="circle-link example-icon-link" svgIcon="twitter"></mat-icon>
    </a>
    <a title="YouTube" href="https://youtube.com/amitshukla_ai" target="_blank" rel="noopener">
      <mat-icon matTooltip="connect with YouTube" class="circle-link example-icon-link" svgIcon="youtube"></mat-icon>
  </a>
  <a title="Medium Blogs" href="https://medium.com/@Amit_Shukla" target="_blank" rel="noopener">
    <mat-icon matTooltip="connect with Medium Blogs" class="circle-link example-icon-link" svgIcon="medium"></mat-icon>
</a>
  </mat-menu>

## File: src/app/ui/aboutus.component.css

**File Content:**



## File: src/app/ui/aboutus.component.html

**File Content:**

<app-header imageUrl="dashboard" pageTitle="About Me"></app-header>
<mat-card>
    <mat-card-subtitle>
        AMIT SHUKLA<br/>
PgMP® PMP® SCM PGDWM B.E. (E&C) B.Sc.<br>
    </mat-card-subtitle>
    <mat-card-content>
        IT Business Intelligence, Analytics SME and Senior Executive Leader <br/><br/>
        Open Source Community Projects
        <button mat-button color="primary" routerLink="/business">by Business</button>
        <button mat-button color="primary" routerLink="/technology">by Technology</button>
        <button mat-button color="primary" routerLink="/pro">Pro</button>
        <br/>
        Build and Share Quality code for Desktop and Mobile App Software development using Web, AI, Machine Learning, Deep Learning algorithms.
        <br/><br/>
        <p>
        Healthcare IT Business Intelligence/Analytics SME, Leader with 20+ years of Functional/IT Consulting and Business leadership experience<br>
        on large scale Business Intelligence, Data warehouse, Analytics, PeopleSoft EPM, Data Mining, BIG DATA (Hadoop, Cloudera, R,<br>
        Julia, Python Machine Learning , Tableau, Trifacta, Arcadia, Cognos, Power BI etc.), Enterprise design Architecture <br><br>
        Cloud computing and Oracle PeopleSoft EPM, ERP (Finance, Supply Chain, HR Analytics and CRM), development, implementations consulting, <br>
        support and Management from conception to completion.<br><br>
        Strong record of success in managing robust IT High Reliable Organizations (HRO).<br><br>
        Proven ability to bring the benefits of IT to solve business issues while delivering applications, solid infrastructure, optimized costs and risks management planning.
        <br><br>
        Provide strategic direction and end-to-end hands-on consulting to senior leadership on technology and management. 
        </p>
    </mat-card-content>
</mat-card>


## File: src/app/ui/business.component.css

**File Content:**



## File: src/app/ui/business.component.html

**File Content:**

<app-header imageUrl="business" pageTitle="Projects by Business"></app-header>
  <mat-card>
    <mat-card-actions>
      <button mat-button color="primary" routerLink="/aboutus">Back</button>
    </mat-card-actions>
      <mat-card-content>
        <mat-accordion>
            <mat-expansion-panel>
              <mat-expansion-panel-header>
                <mat-panel-title>
                    Data Science for ERP
                </mat-panel-title>
                <mat-panel-description>
                    Machine Learning
                </mat-panel-description>
              </mat-expansion-panel-header>
              <mat-list-item *ngFor="let folder of folders">
                <mat-icon mat-list-icon>folder</mat-icon>
                <div mat-line *ngIf="folder.link"><a href="{{ folder.link }}" target="_blank">{{folder.name}}</a></div>
                <div mat-line *ngIf="!folder.link">{{folder.name}} ***WIP</div>
                <div mat-line> {{folder.updated | date}} </div>
              </mat-list-item>
            </mat-expansion-panel>
            <mat-expansion-panel (opened)="panelOpenState = true"
                                 (closed)="panelOpenState = false">
              <mat-expansion-panel-header>
                <mat-panel-title>
                    iOS, Android, PWA
                </mat-panel-title>
                <mat-panel-description>
                  Assets Tracking Apps
                </mat-panel-description>
              </mat-expansion-panel-header>
              <mat-list-item *ngFor="let note of tracking">
                <mat-icon mat-list-icon>note</mat-icon>
                <div mat-line><a href="{{ note.link }}" target="_blank">{{note.name}}</a></div>
                <div mat-line> {{note.updated | date}} </div>
              </mat-list-item>
            </mat-expansion-panel>
            <mat-expansion-panel (opened)="panelOpenState = true"
                                 (closed)="panelOpenState = false">
              <mat-expansion-panel-header>
                <mat-panel-title>
                    iOS, Android, PWA
                </mat-panel-title>
                <mat-panel-description>
                  ERP Apps
                </mat-panel-description>
              </mat-expansion-panel-header>
              <mat-list-item *ngFor="let note of erps">
                <mat-icon mat-list-icon>note</mat-icon>
                <div mat-line><a href="{{ note.link }}" target="_blank">{{note.name}}</a></div>
                <div mat-line> {{note.updated | date}} </div>
              </mat-list-item>
            </mat-expansion-panel>
            <mat-expansion-panel (opened)="panelOpenState = true"
                                 (closed)="panelOpenState = false">
              <mat-expansion-panel-header>
                <mat-panel-title>
                    iOS, Android, PWA
                </mat-panel-title>
                <mat-panel-description>
                  Small Business Apps
                </mat-panel-description>
              </mat-expansion-panel-header>
              <mat-list-item *ngFor="let note of smallbusiness">
                <mat-icon mat-list-icon>note</mat-icon>
                <div mat-line><a href="{{ note.link }}" target="_blank">{{note.name}}</a></div>
                <div mat-line> {{note.updated | date}} </div>
              </mat-list-item>
            </mat-expansion-panel>
            <mat-expansion-panel (opened)="panelOpenState = true"
                                 (closed)="panelOpenState = false">
              <mat-expansion-panel-header>
                <mat-panel-title>
                    iOS, Android, PWA
                </mat-panel-title>
                <mat-panel-description>
                  All Live Apps
                </mat-panel-description>
              </mat-expansion-panel-header>
              <mat-list-item *ngFor="let note of notes">
                <mat-icon mat-list-icon>note</mat-icon>
                <div mat-line><a href="{{ note.link }}" target="_blank">{{note.name}}</a></div>
                <div mat-line> {{note.updated | date}} </div>
              </mat-list-item>
            </mat-expansion-panel>
          </mat-accordion>        
      </mat-card-content>
  </mat-card>

## File: src/app/ui/pro.component.css

**File Content:**



## File: src/app/ui/pro.component.html

**File Content:**

<app-header imageUrl="donut_small" pageTitle="Pro"></app-header>
  <mat-card>
    <mat-card-actions>
      <button mat-button color="primary" routerLink="/aboutus">Back</button>
    </mat-card-actions>
      <mat-card-content>
        <mat-accordion>
            <mat-expansion-panel>
              <mat-expansion-panel-header>
                <mat-panel-title>
                    Data Science for ERP
                </mat-panel-title>
                <mat-panel-description>
                    Machine Learning
                </mat-panel-description>
              </mat-expansion-panel-header>
              <mat-list-item *ngFor="let folder of folders">
                <mat-icon mat-list-icon>folder</mat-icon>
                <div mat-line *ngIf="folder.link"><a href="{{ folder.link }}" target="_blank">{{folder.name}}</a></div>
                <div mat-line *ngIf="!folder.link">{{folder.name}} ***WIP</div>
                <div mat-line> {{folder.updated | date}} </div>
              </mat-list-item>
            </mat-expansion-panel>
            <mat-expansion-panel (opened)="panelOpenState = true"
                                 (closed)="panelOpenState = false">
              <mat-expansion-panel-header>
                <mat-panel-title>
                    iOS, Android, PWA
                </mat-panel-title>
                <mat-panel-description>
                  Live Pro Apps
                </mat-panel-description>
              </mat-expansion-panel-header>
              <mat-list-item *ngFor="let note of notes">
                <mat-icon mat-list-icon>note</mat-icon>
                <div mat-line><a href="{{ note.link }}" target="_blank">{{note.name}}</a></div>
                <div mat-line> {{note.updated | date}} </div>
              </mat-list-item>
            </mat-expansion-panel>
          </mat-accordion>        
      </mat-card-content>
  </mat-card>

## File: src/app/ui/technology.component.css

**File Content:**



## File: src/app/ui/technology.component.html

**File Content:**

<app-header imageUrl="build" pageTitle="Projects by Tech"></app-header>
  <mat-card>
    <mat-card-actions>
      <button mat-button color="primary" routerLink="/aboutus">Back</button>
    </mat-card-actions>
      <mat-card-content>
        <mat-accordion>
            <mat-expansion-panel>
              <mat-expansion-panel-header>
                <mat-panel-title>
                    AI - Machine/Deep Learning
                </mat-panel-title>
                <mat-panel-description>
                    Julia, Python
                </mat-panel-description>
              </mat-expansion-panel-header>
              <mat-list-item *ngFor="let folder of ai">
                <mat-icon mat-list-icon>folder</mat-icon>
                <div mat-line *ngIf="folder.link"><a href="{{ folder.link }}" target="_blank">{{folder.name}}</a></div>
                <div mat-line *ngIf="!folder.link">{{folder.name}} ***WIP</div>
                <div mat-line> {{folder.updated | date}} </div>
              </mat-list-item>
            </mat-expansion-panel>
            <mat-expansion-panel (opened)="panelOpenState = true"
                                 (closed)="panelOpenState = false">
              <mat-expansion-panel-header>
                <mat-panel-title>
                    Angular Firebase
                </mat-panel-title>
                <mat-panel-description>
                  Live Apps
                </mat-panel-description>
              </mat-expansion-panel-header>
              <mat-list-item *ngFor="let note of ngfirebase">
                <mat-icon mat-list-icon>note</mat-icon>
                <div mat-line><a href="{{ note.link }}" target="_blank">{{note.name}}</a></div>
                <div mat-line> {{note.updated | date}} </div>
              </mat-list-item>
            </mat-expansion-panel>
            <mat-expansion-panel (opened)="panelOpenState = true"
                                 (closed)="panelOpenState = false">
              <mat-expansion-panel-header>
                <mat-panel-title>
                    Angular MYSQL
                </mat-panel-title>
                <mat-panel-description>
                  Live Apps
                </mat-panel-description>
              </mat-expansion-panel-header>
              <mat-list-item *ngFor="let note of ngsql">
                <mat-icon mat-list-icon>note</mat-icon>
                <div mat-line><a href="{{ note.link }}" target="_blank">{{note.name}}</a></div>
                <div mat-line> {{note.updated | date}} </div>
              </mat-list-item>
            </mat-expansion-panel>
            <mat-expansion-panel (opened)="panelOpenState = true"
                                 (closed)="panelOpenState = false">
              <mat-expansion-panel-header>
                <mat-panel-title>
                    Angular MongoDB (MEAN)
                </mat-panel-title>
                <mat-panel-description>
                  Live Apps
                </mat-panel-description>
              </mat-expansion-panel-header>
              <mat-list-item *ngFor="let note of ngmongo">
                <mat-icon mat-list-icon>note</mat-icon>
                <div mat-line><a href="{{ note.link }}" target="_blank">{{note.name}}</a></div>
                <div mat-line> {{note.updated | date}} </div>
              </mat-list-item>
            </mat-expansion-panel>
            <mat-expansion-panel (opened)="panelOpenState = true"
                                 (closed)="panelOpenState = false">
              <mat-expansion-panel-header>
                <mat-panel-title>
                    Flutter
                </mat-panel-title>
                <mat-panel-description>
                  Live Apps
                </mat-panel-description>
              </mat-expansion-panel-header>
              <mat-list-item *ngFor="let note of flutter">
                <mat-icon mat-list-icon>note</mat-icon>
                <div mat-line><a href="{{ note.link }}" target="_blank">{{note.name}}</a></div>
                <div mat-line> {{note.updated | date}} </div>
              </mat-list-item>
            </mat-expansion-panel>
            <mat-expansion-panel (opened)="panelOpenState = true"
                                 (closed)="panelOpenState = false">
              <mat-expansion-panel-header>
                <mat-panel-title>
                    All
                </mat-panel-title>
                <mat-panel-description>
                  Live Apps
                </mat-panel-description>
              </mat-expansion-panel-header>
              <mat-list-item *ngFor="let note of all">
                <mat-icon mat-list-icon>note</mat-icon>
                <div mat-line><a href="{{ note.link }}" target="_blank">{{note.name}}</a></div>
                <div mat-line> {{note.updated | date}} </div>
              </mat-list-item>
            </mat-expansion-panel>
          </mat-accordion>        
      </mat-card-content>
  </mat-card>

## File: src/assets/css/styles.css

**File Content:**

@import '~@angular/material/prebuilt-themes/indigo-pink.css';

.small-headline {
  font-family: times, Times New Roman, times-roman, georgia, serif;
  font-size: 8;
  line-height: 40px;
  letter-spacing: -1px;
  color: #0088cc;
}

.middle-headline {
  font-family: times, Times New Roman, times-roman, georgia, serif;
  font-size: 28px;
  line-height: 40px;
  letter-spacing: -1px;
  color: #0088cc;
}

.large-headline {
  font-family: times, Times New Roman, times-roman, georgia, serif;
  font-size: 48px;
  line-height: 40px;
  letter-spacing: -1px;
  color: #0088cc;
  margin: 0 0 0 0;
  padding: 0 0 0 0;
  font-weight: 100;
}

.small-spacer {
  margin-left: 10px;
}

.med-spacer {
  margin-left: 70px;
}

.large-spacer {
  margin-left: 170px;
}

.body {
  background-color: white;
  color: #777777;
  font-family: Arial, Helvetica, sans-serif;
  font-size: 10;
  line-height: 22px;
  margin: 0;
}

html,
body {
  height: 100%;
}

body {
  margin: 0;
  font-family: Roboto, "Helvetica Neue", sans-serif;
}

.circle-link {
  height: 40px;
  width: 40px;
  border-radius: 40px;
  margin: 8px;
  background-color: white;
  border: 1px solid #eeeeee;
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.12), 0 1px 2px rgba(0, 0, 0, 0.24);
  transition: 1s ease-out;
}

.circle-link:hover {
  transform: translateY(-0.25rem);
  box-shadow: 0px 3px 15px rgba(0, 0, 0, 0.2);
}

.example-icon {
  padding: 0 7px;
  color: #777777;
}
.example-icon-link {
  padding: 0 6px;
  color: steelblue;
  background-color: white;
}
.example-icon-link-large {
  color: steelblue;
}
.mat-menu-item {
  padding: 0 7px;
  color: steelblue;
}
.example-spacer {
  flex: 1 1 auto;
}
.example-card {
  /* max-width: 100%; */
  width: 600px;
}
.example-card-fixed {
  max-width: 400px;
}
.example-header-image {
  background-image: url('http://localhost:4200/assets/images/sound.gif');
  background-size: cover;
}
.blueBG {
  background-color: grey;
  width: 400px;
  height: 180px;
}
.example-form {
  min-width: 150px;
  max-width: 500px;
  width: 100%;
}
.example-full-width {
  width: 100%;
}
mat-chip {
  max-width: 200px;
}

## File: src/index.html

**File Content:**

<!doctype html>
<html lang="en">
<head>
  <meta charset="utf-8">
  <title>Amitshukla</title>
  <base href="/">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="icon" type="image/x-icon" href="favicon.ico">
  <link href="https://fonts.googleapis.com/css?family=Roboto:300,400,500&display=swap" rel="stylesheet">
  <link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet">

</head>
<body class="mat-typography">
  <app-root></app-root>
</body>
</html>


## File: src/styles.css

**File Content:**

/* You can add global styles to this file, and also import other style files */

html, body { height: 100%; }
body { margin: 0; font-family: Roboto, "Helvetica Neue", sans-serif; }

:host {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
    font-size: 14px;
    color: #333;
    box-sizing: border-box;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
  }

  h1,
  h2,
  h3,
  h4,
  h5,
  h6 {
    margin: 8px 0;
  }

  p {
    margin: 0;
  }

  .spacer {
    flex: 1;
  }
  .small-spacer {
    width: 5px;
  }
  .toolbar {
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    height: 60px;
    display: flex;
    align-items: center;
    background-color: #1976d2;
    color: white;
    font-weight: 600;
  }

  .toolbar img {
    margin: 0 16px;
  }

  .toolbar #twitter-logo {
    height: 40px;
    margin: 0 16px;
  }

  .toolbar #twitter-logo:hover {
    opacity: 0.8;
  }

  .content {
    display: flex;
    margin: 82px auto 32px;
    padding: 0 16px;
    max-width: 960px;
    flex-direction: column;
    align-items: center;
  }

  svg.material-icons {
    height: 24px;
    width: auto;
  }

  svg.material-icons:not(:last-child) {
    margin-right: 8px;
  }

  .card svg.material-icons path {
    fill: #888;
  }

  .card-container {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    margin-top: 16px;
  }

  .card {
    border-radius: 4px;
    border: 1px solid #eee;
    background-color: #fafafa;
    height: 40px;
    width: 200px;
    margin: 0 8px 16px;
    padding: 16px;
    display: flex;
    flex-direction: row;
    justify-content: center;
    align-items: center;
    transition: all 0.2s ease-in-out;
    line-height: 24px;
  }

  .card-container .card:not(:last-child) {
    margin-right: 0;
  }

  .card.card-small {
    height: 16px;
    width: 168px;
  }

  .card-container .card:not(.highlight-card) {
    cursor: pointer;
  }

  .card-container .card:not(.highlight-card):hover {
    transform: translateY(-3px);
    box-shadow: 0 4px 17px rgba(0, 0, 0, 0.35);
  }

  .card-container .card:not(.highlight-card):hover .material-icons path {
    fill: rgb(105, 103, 103);
  }

  .card.highlight-card {
    background-color: #1976d2;
    color: white;
    font-weight: 600;
    border: none;
    width: auto;
    min-width: 30%;
    position: relative;
  }

  .card.card.highlight-card span {
    margin-left: 60px;
  }

  svg#rocket {
    width: 80px;
    position: absolute;
    left: -10px;
    top: -24px;
  }

  svg#rocket-smoke {
    height: calc(100vh - 95px);
    position: absolute;
    top: 10px;
    right: 180px;
    z-index: -10;
  }

  a,
  a:visited,
  a:hover {
    color: #1976d2;
    text-decoration: none;
  }

  a:hover {
    color: #125699;
  }

  .terminal {
    position: relative;
    width: 80%;
    max-width: 600px;
    border-radius: 6px;
    padding-top: 45px;
    margin-top: 8px;
    overflow: hidden;
    background-color: rgb(15, 15, 16);
  }

  .terminal::before {
    content: "\2022 \2022 \2022";
    position: absolute;
    top: 0;
    left: 0;
    height: 4px;
    background: rgb(58, 58, 58);
    color: #c2c3c4;
    width: 100%;
    font-size: 2rem;
    line-height: 0;
    padding: 14px 0;
    text-indent: 4px;
  }

  .terminal pre {
    font-family: SFMono-Regular,Consolas,Liberation Mono,Menlo,monospace;
    color: white;
    padding: 0 1rem 1rem;
    margin: 0;
  }

  .circle-link {
    height: 40px;
    width: 40px;
    border-radius: 40px;
    margin: 8px;
    background-color: white;
    border: 1px solid #eeeeee;
    display: flex;
    justify-content: center;
    align-items: center;
    cursor: pointer;
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.12), 0 1px 2px rgba(0, 0, 0, 0.24);
    transition: 1s ease-out;
  }

  .circle-link:hover {
    transform: translateY(-0.25rem);
    box-shadow: 0px 3px 15px rgba(0, 0, 0, 0.2);
  }

  footer {
    margin-top: 8px;
    display: flex;
    align-items: center;
    line-height: 20px;
  }

  footer a {
    display: flex;
    align-items: center;
  }

  .github-star-badge {
    color: #24292e;
    display: flex;
    align-items: center;
    font-size: 12px;
    padding: 3px 10px;
    border: 1px solid rgba(27,31,35,.2);
    border-radius: 3px;
    background-image: linear-gradient(-180deg,#fafbfc,#eff3f6 90%);
    margin-left: 4px;
    font-weight: 600;
    font-family: -apple-system,BlinkMacSystemFont,Segoe UI,Helvetica,Arial,sans-serif,Apple Color Emoji,Segoe UI Emoji,Segoe UI Symbol;
  }

  .github-star-badge:hover {
    background-image: linear-gradient(-180deg,#f0f3f6,#e6ebf1 90%);
    border-color: rgba(27,31,35,.35);
    background-position: -.5em;
  }

  .github-star-badge .material-icons {
    height: 16px;
    width: 16px;
    margin-right: 4px;
  }

  svg#clouds {
    position: fixed;
    bottom: -160px;
    left: -230px;
    z-index: -10;
    width: 1920px;
  }


  /* Responsive Styles */
  @media screen and (max-width: 767px) {

    .card-container > *:not(.circle-link) ,
    .terminal {
      width: 100%;
    }

    .card:not(.highlight-card) {
      height: 16px;
      margin: 8px 0;
    }

    .card.highlight-card span {
      margin-left: 72px;
    }

    svg#rocket-smoke {
      right: 120px;
      transform: rotate(-5deg);
    }
  }

  @media screen and (max-width: 575px) {
    svg#rocket-smoke {
      display: none;
      visibility: hidden;
    }
  }

