# Node Strategy Pattern

This is an example of how to work with different strategies in NodeJS. In this demo, you will find various strategies for handling user login scenarios.

1. **Agree to a contract.** There is no such thing as a concrete interface in JavaScript, so we are required to use our own discipline to enforce a contract. In our case, all strategies will have a `login(username, password)` function and return a promise. The promise result, at the very least, will have a `success` property.
2. **Register strategies.** All strategies are registered in `loginStrategies/index.js`.

The advantage of this way of writing strategies is that the mainline of code does not need to be altered in any way when adding a new strategy at a later date. We avoid nasty `if/else` conditions in our code.

### Using Test

In the test environment, the user password is always "test". We can use any user we want from our database in `users.json`.

```
$ NODE_ENV=test node app.js alice test
```

```json
{
  "stragegy": "test",
  "success": true,
  "username": "alice"
}
```

### Using Active directory

We are using a test active directory server located at [ForumSys](http://www.forumsys.com/en/tutorials/integration-how-to/ldap/online-ldap-test-server/).

The list of valid users is: einstein, euclid, euler, gauss, newton, riemann, tesla. The password for all users is "password".

```
$ LOGIN_METHOD=AD node app.js riemann password
```

```json
{
  "dn": "uid=riemann,dc=example,dc=com",
  "host": "ldap.forumsys.com",
  "port": 389,
  "success": true,
  "strategy": "active directory",
  "objectName": "uid=riemann,dc=example,dc=com",
  "entry": {
    "dn": "uid=riemann,dc=example,dc=com",
    "controls": [],
    "userPassword": "{sha}W6ph5Mm5Pz8GgiULbPgzG37mj9g=",
    "objectClass": [
      "inetOrgPerson",
      "organizationalPerson",
      "person",
      "top"
    ],
    "cn": "Bernhard Riemann",
    "sn": "Riemann",
    "uid": "riemann",
    "mail": "riemann@ldap.forumsys.com"
  }
}
```

### Using Database

The list of users and passwords can be found in `users.json`.

```
$ LOGIN_METHOD=DB node app.js eleanor h@sh1ng
```

```json
{
  "success": true,
  "strategy": "database",
  "username": "eleanor",
  "id": 5
}
```
