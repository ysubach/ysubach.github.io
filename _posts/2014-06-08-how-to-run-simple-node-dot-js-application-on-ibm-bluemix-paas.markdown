---
layout: post
title: "How to run simple Node.js application on IBM BlueMix PaaS"
tags: [nodejs, ibm, bluemix] 
---
[Codename: BlueMix](http://bluemix.net) (or just BlueMix) is Platform as-a-Service solution created by IBM. It's built on top of SoftLayer IaaS also owned by IBM. BlueMix is opened for beta testing and currently free.

In this post I'll tell you how to create simple Node.js application and link it to the Git repository. As result you'll be able to deploy your application updates to the BlueMix app instance by calling `git push`. It is very convenient for incremental updates.

Let's start.

#### 1) Create BlueMix account

First thing first: go to [http://bluemix.net/](http://bluemix.net/) and create free account.

After that login and you'll see _BlueMix Dashboard_ which is similar to my screenshot:

![Create App](/assets/blog-2014/bluemix-nh/create-app.png)

#### 2) Create new Node.js app

Click _"Create an Application"_ button and select _"Node.js"_ runtime:

![Create app popup](/assets/blog-2014/bluemix-nh/create-app-popup.png)

Click _"Create App"_ and choose your app name:

![Create app name](/assets/blog-2014/bluemix-nh/create-app-name.png)

Now return to Dashboard and see you app there. Give it some time to be started and once you see green light and "running" status visit your URL:

[http://ys-nodejs-hello.ng.bluemix.net/](http://ys-nodejs-hello.ng.bluemix.net/)

It will show "Welcome to BlueMix!" page. Cool!

#### 3) Link app to Git repository

Next step is linking to Git repository. Click on your application box and app details. There is _"Add Git integration"_ link:

![Add git](/assets/blog-2014/bluemix-nh/add-git.png)

It will ask you to create Jazz account, if you here for the first time. As result you'll see:

![Add git result](/assets/blog-2014/bluemix-nh/add-git-result.png)

Click "Continue" and let's move to next step.

#### 4) Make changes via Git

Now your application is placed into private Git repository, integrated with BlueMix. You can get this app to your work machine:

```
git clone https://hub.jazz.net/git/ysubach/ys-nodejs-hello
cd ys-nodejs-hello
npm install
node app.js
```

Now open [http://localhost:3000/](http://localhost:3000/) and you should see "Welcome to BlueMix!" page.

Let's add new cool function to our application, open `app.js` file in your favorite text editor and add following lines after `app.get('/' ...` call:

``` javascript 
app.get('/test', function(req, res) {
  res.send('Test works!');
})
```

Restart your app using `node app.js` command and open [http://localhost:3000/test](http://localhost:3000/test) URL. If all ok, then you'll see `Test works!` message.

The final step is very simple. Just commit your changes to Jazz repository and open your BlueMix application URL:

```
git add app.js
git commit -m 'added /test handler'
git push origin master
```

BlueMix URL for our new function: [http://ys-nodejs-hello.ng.bluemix.net/test](http://ys-nodejs-hello.ng.bluemix.net/test)

I hope you like it! 
