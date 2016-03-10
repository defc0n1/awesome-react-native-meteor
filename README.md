# Awesome React Native Meteor
An "awesome" type curated list of how to use React Native and Meteor together. Based on [Awesome React Native](https://github.com/jondot/awesome-react-native) which was based on [this](https://github.com/avelino/awesome-go), which was based on [this](https://github.com/vinta/awesome-python)...

### React Native & Meteor - why, how, what?
**React Native** - is responsible for the native apps
**Meteor** is the server, providing the methods, account system etc.

#### Why do they work well together?
* **JS everywhere**  - React Native apps are written in JS. So are Meteor apps. This means more code reuse.
* **Both use NPM** - from Meteor 1.3, there's full [NPM support](https://medium.com/@borellvi/meteor-meets-npm-a5cc48d90abe#.fodwjxha3).
* **Easy backend set up** - Meteor has an awesome built-in accounts system and much more. You can set up a decent backend in a few lines of code.

Each has its own set of benefits.

#### How do they communicate?
Meteor sends data via its proprietary [DDP](https://www.meteor.com/ddp) protocol.

This is used to `call` methods on the server and `subscribe` to data.

## Tutorials / Writeups
* [Meteor + React Native. Learning from Experience.](http://blog.differential.com/meteor-react-native-learning-from-experience/) - Writeup overcoming various challenges of porting

## Example apps
* [Todos](https://github.com/spencercarli/meteor-todos-react-native) - Meteor todos wired up to React Native app
* [Friends](https://github.com/JustMeteor/friends) - Simple app displaying list in collection. [Writeup here](http://justmeteor.com/blog/friends-connecting-meteor-and-react-native-by-example/)
* [Scientists](https://github.com/hharnisc/react-native-meteor-websocket-polyfill) - Simple app demonstrating DDP functionality

## Packages
### [react-native-meteor](https://github.com/inProgress-team/react-native-meteor)
A 'magic included' easy DDP connection exposing `subscribe`, `call`, `loginWithPassword` as well as `getMeteorData()` to use inside React components.

```
@connectMeteor
export default class App extends Component {
  componentWillMount() {
    const url = 'http://192.168.X.X:3000/websocket';
    Meteor.connect(url);
  }
  startMeteorSubscriptions() {
    Meteor.subscribe('todos');
    Meteor.subscribe('settings');
  }
  getMeteorData() {
    return {
      settings: Meteor.collection('settings').findOne()
    };
  }
  renderRow(todo) {
    return (
      <Text>{todo.title}</Text>
    );
  }
  render() {
    const { settings } = this.data;

    <View>
      <Text>{settings.title}</Text>
        <MeteorListView
          collection="todos"
          selector={{done: true}}
          options={{sort: {createdAt: -1}}}
          renderItem={this.renderRow}
        />
    </View>

  }
}
```


### [node-ddp-client](https://github.com/hharnisc/node-ddp-client/tree/minimongo-cache)
A more bare-bones package. It exposes `call` and `subscribe`. (Make sure to use the `minimongo-cache` branch).

Implemented [https://github.com/hharnisc/react-native-meteor-websocket-polyfill](https://github.com/hharnisc/react-native-meteor-websocket-polyfill) and [here](https://github.com/spencercarli/meteor-todos-react-native).

The methods are elegantly wrapped in Promises in [todos](https://github.com/spencercarli/meteor-todos-react-native).

```
componentWillMount() {
  MessagesDB.subscribe(0, MESSAGES_INTERVAL)
    .then(() => {
      MessagesDB.observe((messages) => {
        this.setState({
          messages: messages
        });
      });
    })
    .then(() => {
      return MessagesDB.messageCount();
    })
    .then((r) => {
      this.setState({
        totalMessageCount: r
      })
    })
    .catch((err) => {
      console.log('Error: ', err);
    })
},
```

## Videos
* [Harrison Harnisch - React Native and Meteor - Devshop SF](https://www.youtube.com/watch?v=7BF5LHn2B5s)

### Why not use Meteor + Ionic + Cordova?
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
