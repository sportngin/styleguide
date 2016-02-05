---
layout: page
title: Angular
header: Angular Style Guide
group: navigation
---
{% include JB/setup %}

This is a list of style choices and idioms we wish to apply when using AngularJs.

## Access to Parent Scope

When a parameter is assigned in a template, it will sometimes need explicit access to the parent scope due to new child scopes being created by the parent elements in the template. For example:

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

This naming convention for form elements is fully supported by Angular, and allows us to explicitly define which scope object the new element should be stored on.
