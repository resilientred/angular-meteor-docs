# blaze-template directive

Included with the `urigo:angular-blaze-template` [package](https://github.com/Urigo/angular-blaze-template)

A directive to include Meteor native Blaze [templates](http://docs.meteor.com/#/full/templates_api)

Holds the current Angular [scope](https://docs.angularjs.org/guide/scope) as [Template.currentData](http://docs.meteor.com/#/full/template_currentdata).

----

## Usage

    meteor add urigo:angular-blaze-template

    <blaze-template
        name="">
    </blaze-template>

### Arguments

<table class="variables-matrix input-arguments">
<thead>
<tr>
  <th>Param</th>
  <th>Type</th>
  <th>Details</th>
  <th>Required</th>
</tr>
</thead>
<tbody>
<tr>
  <td>Template name</td>
  <td><a href="" class="label type-hint type-hint-string">string</a></td>
  <td><p>The name of the template to include</p></td>
  <td><a href="" class="label type-hint type-hint-array">Yes</a></td>
</tr>
</tbody>
</table>

----

### Simple Example


    <blaze-template name="loginButtons"></blaze-template>

----

### Using Angular Scope inside Meteor template


The `blaze-template` directive holds the Angular scope of the directive as the
as [Template.currentData](http://docs.meteor.com/#/full/template_currentdata) of the Meteor template.


#### Example using scope data


__`blaze-angular.html`:__

    <head></head>
    <body ng-app="blaze-angular-scope"
          ng-controller="AngularCtrl"
          ng-include="'blaze-angular-scope.ng.html'">
    </body>

    <template name="partiesBlazeTemplate">
      <div>
        Angular parties inside Blaze:
        {{dstache}}#each parties}}
          {{dstache}}> party}}
        {{dstache}}/each}}
      </div>
    {{lt}}/template>

    <template name="party">
      <li>Name: {{dstache}}name}} , Description: {{dstache}}description}}</li>
    {{lt}}/template>

__`blaze-angular-scope.ng.html`:__

    <h1>Inside Angular</h1>

    <form>
      <label>Name</label>
      <input ng-model="newParty.name">
      <label>Description</label>
      <input ng-model="newParty.description">
      <button ng-click="parties.push(newParty); newParty='';">Add</button>
    </form>

    <blaze-template name="partiesBlazeTemplate"></blaze-template>

__`blaze-angular.js`:__

    if (Meteor.isClient) {

      angular.module('blaze-angular-scope',['angular-meteor']);

      angular.module("blaze-angular-scope").controller("AngularCtrl", ['$scope',
        function($scope){

          $scope.parties = [
            {'name': 'Dubstep-Free Zone',
             'description': 'Can we please just for an evening not listen to dubstep.'},
            {'name': 'All dubstep all the time',
             'description': 'Get it on!'}
          ];

      }]);

      Template.partiesBlazeTemplate.helpers({
        parties: function() {
          return Template.currentData().getReactively('parties', true);
        }
      });
    }



### Passing arguments to blaze-template

To pass parameters to a `blaze-template` directive:

1. Create a new Blaze template that includes the template you want to include
2. Add the parameters to the new template
3. Include that new template with blaze-template


    <blaze-template name="quickFormWithParameters"></blaze-template>
