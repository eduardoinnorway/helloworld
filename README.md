[![Stories in Ready](https://badge.waffle.io/melvincarvalho/helloworld.png?label=ready&title=Ready)](https://waffle.io/melvincarvalho/helloworld)
# helloworld

[![Join the chat at https://gitter.im/melvincarvalho/helloworld](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/melvincarvalho/helloworld?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

#hello world for SoLiD apps

Running live here: https://melvincarvalho.github.io/helloworld/

Quick Start for contributors
----------------------------

```
$ git clone git://github.com/melvincarvalho/helloworld
$ cd helloworld
$ sudo npm -g install bower
$ bower install
```

## Tutorial

In this tutorial we will cover how to build a simple client side hello world app using the SoLiD framework.  

*What you will learn*

* How to create your first client side SoLiD app
* How to use decentralized login and logout
* How to to delegate HEAD requests to identify using the [User](https://www.w3.org/community/rww/wiki/User_Header) header
* How to use notifcations using [Lumx](http://ui.lumapps.com/) and [AngularJS](https://angularjs.org/)

## The App

SoLiD apps typically run completely client side and do not require a server, except to identify a user, or store data.

Hello world is a simple app that allows decentralized login and logout using the [WebID](http://webid.info/) Identity system.  The remote server will perform the authentication, typically using TLS, but any authentication is allowed provided that the [User](https://www.w3.org/community/rww/wiki/User_Header) header is sent back.

First, we will look at how to login using JavaScript.  The **login** code is below:

```javascript
    $http({
      method: 'HEAD',
      url: AUTHENDPOINT,
      withCredentials: true
    }).success(function(data, status, headers) {
      var user = headers('User');
      if (user && user.length > 0 && user.slice(0,4) == 'http') {
        LxNotificationService.success('Login Successful!');
        $scope.loggedIn = true;
        $scope.user = user;
      } else {
        LxNotificationService.error('WebID-TLS authentication failed.');
        console.log('WebID-TLS authentication failed.');
      }
      $scope.loginTLSButtonText = 'Login';
    }).error(function(data, status, headers) {
      LxNotificationService.error('Could not connect to auth server: HTTP '+status);
      console.log('Could not connect to auth server: HTTP '+status);
      $scope.loginTLSButtonText = 'Login';
    });

  ```

The system used is a delegated authentication.  This is because a server is required in order to verify who a user is.

A HEAD request to any WebID enabled server will return a User: header telling who is using the app.  That user can then be used to cusomize the app.  In our case we simply set `$scope.user`.

The withCredentials flag is set to true in order to prevent a CORS error.

The AUTHENDPOINT in our example is set to : https://databox.me/

```javascript
    AUTHENDPOINT = "https://databox.me/";
```

The **logout** code simply unsets the `$scope.loggedIn` variable:

```javascript
  $scope.logout = function() {
    LxNotificationService.success('Logout Successful!');
    $scope.loggedIn = false;
  };
```

Putting this simple functionality together in using the [AngularJS](https://angularjs.org/) framework (with [lumx](http://ui.lumapps.com/) extensions) it is possible to create a simple demo:

  [Live Demo](http://melvincarvalho.github.io/helloworld/)

## Summary

In this chapter we have shown how to identify and verify a user using delegated login and WebID.  We have packaged the code into an [AngularJS](https://angularjs.org/) app and presented a simple demo.  In the next chapter we will show how to save data to your Personal Online Data Store (Pod).

## See Also

* [Source Code](https://github.com/melvincarvalho/helloworld)
* [Live Demo](http://melvincarvalho.github.io/helloworld/)
* [WebID](http://webid.info/)
* [AngularJS](https://angularjs.org/)
* [Lumx](http://ui.lumapps.com/)
* [User Header](https://www.w3.org/community/rww/wiki/User_Header)
