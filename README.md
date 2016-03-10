# Awesome React Native Meteor
An "awesome" type curated list of how to use React Native and Meteor together.

### React Native & Meteor - why, how, what?
#### How do they work together?
**React Native** - is responsible for the native apps
**Meteor** is the server, providing the methods, account system etc.

#### Why do they work well together?
* **JS everywhere**  - React Native apps are written in JS. So are Meteor apps. This means more code reuse.
* **Easy backend set up** - Meteor has an awesome built-in accounts system and much more. You can set up a decent backend in a few lines of code.

Additionally, there are many benefits of using Meteor as a backend such as free dev hosting etc.

#### How do they communicate?
Meteor sends data via its proprietary [DDP](https://www.meteor.com/ddp) protocol.

This is used to `call` methods on the server and `subscribe` to data.

#### Why not use Meteor + Ionic + Cordova?
It's been possible to build PhoneGap / Cordova apps for a while. You use the frontend library [Ionic](http://ionicframework.com/) with Meteor's [Blaze](https://www.meteor.com/blaze) which have been tied together nicely by [Meteoric](http://meteoric.github.io/).

If you're trying to decide between React Native and Cordova, make sure to read up on the grand old [hybrid vs. native debate](https://www.google.de/search?q=hybrid+vs+native&oq=hybrid+vs+native&aqs=chrome..69i57l2j69i60j69i61j69i60j69i61.2722j0j1&sourceid=chrome&es_sm=119&ie=UTF-8).

**Advantages**
* More frontend code reuse (especially if your app is written with Blaze)
* Gentler learning curve / more familiar technologies

**Disadvantages**
* Some worrying bugs, like very long time top open app
* Many believe [Blaze](https://www.discovermeteor.com/blog/blaze-react-meteor/) is on the way out
* Ionic was designed to work with Angular. Meteoric is a hack (albeit a very good one).

Our 10 cents:
* If the user experience has to be really slick, use React Native. If not (e.g. B2B apps) Meteoric is probably ok.
* Prototyping quickly -> Meteoric + Cordova. Building for the long haul -> React Native

### Gotchas
* React Native is a way less smoothe developer experience than Meteor. Be prepared for lots of red angry error screens
* Do not use arrow functions `() => ` to define `Meteor.publish` functions or `Meteor.methods({})`, otherwise `this` will not behave as expected
