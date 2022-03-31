---
layout: post
title: "Authentication in Rails"
tag: [ruby, rails,authentication]
---

How do you implement authentication for a Rails application?
In this article, I will note only the general ideas. Basically, there are 3 cases:

## Server side rendering application

This is the most basic case. Just simply use [Devise](https://github.com/heartcombo/devise)

## API-mode for web frontend only

When you use API-mode, Devise will not work out of the box, because it [requires cookies](https://github.com/heartcombo/devise#rails-api-mode) while Rails API-mode does not include the middlewares to handle session and cookies. There are 2 choices here: use token or add back cookies middlewares to use Devise. If web frontend is the only consumer of your API, it will be easier to [add back middlewares](https://guides.rubyonrails.org/api_app.html#using-session-middlewares) and setup Devise to work. And using cookies is more secure than using token, because of 2 reasons. First, cookies does not contain user's information in it. It is only a key to unlock the information stored in session on the server. Second, cookies has its own security mechanism, which prevents other website of different domains running in the same browser from accessing cookies' content, while if you use token, you have to store it in local storage, which can be accessed by javascript from other websites.

## API-mode for mobile apps or server calling

Mobile apps and servers do not have cookies, so we cannot use Devise's session authentication in this case. Instead, we can use JWT-based authentication. Basically, when a user login with username and password, you will find and encode user's information (mainly user_id) into a token, then pass it to the client. The client will store that token, and add it into the header of every subsequent requests to server. Then, the server will decode the token to get user_id to query the current logging in user. You can use [jwt gem](https://github.com/jwt/ruby-jwt) to implement the encode-decode logic.
