# subscribe

This method provides the support for `Meteor.subscribe`.

The method expect the first parameter to be the publication name (as defined in the server).

The second argument is a function, which returns an array of arguments as defined in the publication on the server side.
Those arguments can be reactive with the help of [getReactively](/api/1.3.6/get-reactively).

The third parameter is an optional callback which operates exactly like the callbacks in
[Meteor.subscribe](http://docs.meteor.com/#/full/meteor_subscribe).

The `subscribe` method is part of the `ReactiveContext`, and available on every `context` and `$scope` and will stop
automatically when it's context ($scope) is destroyed.

> Tip: You can just use the first argument, the rest is optional.

------

### Arguments

<table class="variables-matrix input-arguments">
  <thead>
  <tr>
    <th>Name</th>
    <th>Type</th>
    <th>Details</th>
    <th>Required</th>
  </tr>
  </thead>
  <tbody>
  <tr>
    <td><strong>Publication name</strong></td>
    <td>
      <a href="" class="label type-hint type-hint-string">String</a>
    </td>
    <td>The name of the publication, as defined in the server side using `Meteor.publish`.</td>
    <td>Yes</td>
  </tr>
  <tr>
    <td>Arguments Function</td>
    <td>
      <a href="" class="label type-hint type-hint-function">Function</a>
    </td>
    <td>A function, which expected to return an array of arguments that will passed to the subscription.</td>
    <td>No</td>
  </tr>
  <tr>
    <td>Result Callback</td>
    <td>
      <a href="" class="label type-hint type-hint-function">Function</a><br /><a href="" class="label type-hint type-hint-object">Object</a>
    </td>
    <td>May include `onStop`, `onReady` and `onStart` callbacks. If there is an error, it is passed as an argument to `onStop`. If a function is passed instead of an object, it is interpreted as an `onReady` callback. The angular-meteor specific `onStart` callback let you know when the subscription is being called.</td>
    <td>No</td>
  </tr>
  </tbody>
</table>

### Return value

The method returns exactly the same parameters as [Meteor.subscribe](http://docs.meteor.com/#/full/meteor_subscribe).

-------

### Usage Example:

    myModule.controller('MyCtrl', ['$scope', '$reactive', function($scope, $reactive) {
      $reactive(this).attach($scope);

      this.subscribe('users');
    }]);


### Example with `getReactively`:

    myModule.controller('MyCtrl', ['$scope', '$reactive', function($scope, $reactive) {
      $reactive(this).attach($scope);

      this.relevantId = 10;
      this.numberOfUsers = 3;

      this.subscribe('users', () => {
        return [ numberOfUsers, this.getReactively('relevantId') ];
      });

      this.relevantId = 50; // This will cause the subscribe arguments method to run again
      this.numberOfUsers = 5; // This won't cause the subscribe arguments method to run again
    }]);

### Example with subscription handle and callbacks:

    myModule.controller('MyCtrl', ['$scope', '$reactive', function($scope, $reactive) {
      $reactive(this).attach($scope);

      this.numberOfParties = 3;

      var subscriptionHandle = this.subscribe('parties', () => [this.numberOfParties], {
        onStart: function () {
          console.log("New subscribtion has been started");
        },
        onReady: function () {
          console.log("onReady And the Items actually Arrive", arguments);
          subscriptionHandle.stop();  // Stopping the subscription, will cause onStop to fire
        },
        onStop: function (error) {
          if (error) {
            console.log('An error happened - ', error);
          } else {
            console.log('The subscription stopped');
          }
        }
      });

      console.log('subscription Handle',
        subscriptionHandle); // Will include subscriptionId, ready() and stop()

    }]);
