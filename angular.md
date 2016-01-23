---
layout: page
title: Angular
header: Angular Style Guide
group: navigation
---
{% include JB/setup %}

This is a list of style choices and idioms we wish to apply when using AngularJs.

## Acces to Parent Scope

When a parameter is assigned in a template, it will sometimes need acess to the parent scope:

```html
  <directive-element>
    <div ng-if="cond">
      <form name="formName">
        ...
      </form>
    </div>
  </directive-element>
```

This will fail to give access to `formName` in the `directiveElement` controller. To expose the parent scope to the child directive we can do the following:

#### _directiveElement.js_

```javascript
  $scope.scope = $scope
```

#### _directiveElement.html_

```html
  <directive-element>
    <div ng-if="cond">
      <form name="scope.formName">
        ...
      </form>
    </div>
  </directive-element>
```

