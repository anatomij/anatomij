---
title:  "How to use Passport.js in express 4?"
date:   2016-05-03 7:23:00
categories: [jekyll]
tags: [javscript]
---



Managing authentication is not a task for the weak, especially if you are not going to use something like Auth0 that stores all the sensitive information on it's own database. If you are brave enough to store all the user data and hashes for passwords in your own database, then Passport.js is a great tool to achieve success in this field.

There are three main strategies to setup and use with Passport.js, one for user signup, one for user signin and another for authentication based on a jwt token.
During signup and signin Passport.js generates a jwt encoded token that gets returned back to the client for further client side authentication and subsequent calls to an API route that requires authentication. For this example we are going to use MongoDB with mongoose to manage incoming user data.

Below are the example routes you would have when authenticated users:

``` ruby
app.post('/api/users/signup', passport.authenticate('signup'), handler.signup);
app.post('/api/users/signin', passport.authenticate('signin'), handler.signin);
app.get('/api/users', passport.authenticate('jwt'), handler.getProfileInfo);

```

Now that we established a route, let's examine the signup strategy for Passport.js:

``` ruby
module.exports = function (passport) {
	passport.use('signup', new LocalStrategy({
    passReqToCallback: true
  }, function (req, username, password, done) {
    User.findOne({username: username})
      .then(function (user) {
        if (user) {
          return done(new Error('User already exist!'));
        } else {
          var newUser = new User(req.body);
          newUser.save(function (err) {
            if(err) {
              return done(err);
            }
            return done(null, newUser);
          });
        }
      })
    }));
}

```
Using the passReqToCallback value to be able to pass in the req object where our user data is stored, we are able to check if the user exists and further save the user.
Notice that we are using Passport's done function to call done on the newUser generated.

Further, we will examine how Passport is able to establish this magic and persist user login sessions. Below is the init.js file we created which is used by Passport to serialize and deserialize the user as it's being passed into our local strategies. Also note the passport object that is being passed into the top function as well as the three local strategies that we setup for authentication of signin, signup and API calls.


``` ruby
module.exports = function(passport){

  passport.serializeUser(function(user, done) {
    console.log('serializing user: ');
    done(null, user._id);
  });

  passport.deserializeUser(function(id, done) {
    User.findById(id, function(err, user) {
        console.log('deserializing user:', user);
        done(err, user);
    });
  });

  signin(passport);
  signup(passport);
  jwt(passport);
}
```

Below is a visual representation of what the functions achieve.

``` ruby

  passport.serializeUser(function(user, done) {
      done(null, user.id);
                   |
  });              |
                   |
                   |____________________> saved to session req.session.passport.user = {id:'..'}
                                     |          
  passport.deserializeUser(function(id, done) {
                    ________________|
                    |
      User.findById(id, function(err, user) {
          done(err, user);
                     |______________>user object attaches to the request as req.user

   });
    });

```
