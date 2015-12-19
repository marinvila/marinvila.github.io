---
title: Foundation Apps.
layout: post
---
Templating engine based on angular.js. This post is aimed at depicting some trivial solutions that might arise developing frontend with Foundation for Apps.

####Links and Libraries

[official Foundation for Apps docs](http://foundation.zurb.com/apps/docs/#!/)

[official Template](https://github.com/zurb/foundation-apps-template/tree/v1.1.0)

[datepicker](https://github.com/Eonasdan/bootstrap-datetimepicker)

####Use a custom controller

    ---
    name: someName
    url: /someUrl
    title: Very Important Page
    **controller: MyCtrl**
    ---

`MyCtrl` will then extend Foundations `DefaultController`.

```javascript
MyCtrl.inject = ['$scope', '$stateParams', '$state', '$controller', '$location'];
function MyCtrl($scope, $stateParams, $state, $controller, $location) {
  angular.extend(this, $controller('DefaultController', {$scope: $scope, $stateParams: $stateParams, $state: $state}))
...
```

Add the controller to the `application` [module](https://github.com/zurb/foundation-apps-template/blob/v1.1.0/client/assets/js/app.js#L4),
and if in separate file, this file will need to be
[added to the gulp task](https://github.com/zurb/foundation-apps-template/blob/v1.1.0/gulpfile.js#L47).
Addition to `gulpfile.js` will require `gulp restart`.

####URL

Information in query parameters

    url: /path/to/start?sector

As a part of path

    url: /path/to/start/:sector

value passed this way will end up in

    $stateParams.sector

With the help of `encodeURI(JSON.stringify(object))` `JSON.parse(decodeURI(object))`

From JS call this route with: `$state.go("someName", {parameterName: parameterValue});`.

Use `ui-sref="someName({parameterName: parameterValue})` in HTML.


####Navigating in JavaScript

modal close // open

    FoundationApi.publish('someModal', 'close'); // 'open'
    FoundationApi.closeActiveElements('someModal')

###Some Angular

Generate more than one element per iteration of `ng-repeat`, `filter`, then `limitTo`

```html
<td data-ng-repeat-start="obj in someArray | filter item.isEnabled | limitTo:3">
{% raw %}{{obj.index}}{% endraw %}
</td>
<td data-ng-repeat-middle>{% raw %}{{cp.key}}{% endraw %}</td>
<td data-ng-repeat-end>{% raw %}{{cp.value}}{% endraw %}</td>
```

Include template

```html
<div ng-include="" src="'path/to/template.html'"></div>
```

Conditional class

```html
<a data-ng-class="heavyButton ? 'alert' : 'secondary'"
    class="button">
{% raw %}  {{heavyButton ? "Press With Caution" : "Just Press Already"}}{% endraw %}
</a>
```