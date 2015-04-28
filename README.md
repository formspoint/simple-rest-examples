# Examples of using [simple:rest](https://github.com/stubailo/meteor-rest/blob/master/packages/rest/README.md#example-code-with-jquery)

A simple example of exposing and consuming a REST API from the standard Todos example app. The API is consumed from a basic React.js web app that is a single static HTML file.

### Running the example

```sh
git clone https://github.com/stubailo/simple-rest-examples.git
cd rest-client
open todos.html
cd ../todos
meteor
```

### Modifications to the Todos app

1. Added the `simple:rest`, `simple:rest-accounts-password`, and `simple:json-routes` packages
2. Modified `server/publish.js` to add cross-origin friendly HTTP headers, and a fancy URL for the publication

### Things to note about the client app

The client app is a basic HTML/JavaScript React.js app, with no server side component at all. It can call methods on the running Meteor app, and has a simple helper function to facilitate that:

```js
// Helper function
var callMethod = function (methodName, data, callback) {
  $.ajax(server + methodName, {
    method: "post",
    data: JSON.stringify(data),
    contentType: "application/json",
    success: function (data) {
      callback(null, data);
    },
    error: function (data) {
      callback(data.responseJSON);
    }
  });
}

// Calling a method
callMethod("methods/todos/update", [
  {_id: id},
  { $set: {
      checked: newCheckedValue
  } }
]);
```

There is also a super simple demo of login functionality:

```js
// Calling login method
callMethod("users/login", loginOptions, function (err, res) {
  if (err) {
    self.setState({
      errorMessage: err.reason
    });
  } else {
    self.props.onLoggedIn(res);
  }
});
```

Currently, the access token is not saved anywhere, so when you refresh the page the login doesn't persist.
