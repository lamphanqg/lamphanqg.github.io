---
layout: post
title: "Initiate a Rails project with Webpack in Docker"
tag: [ruby, rails, docker]
---
# Rails with Webpack

From v5.1, Ruby on Rails has supported Webpack out-of-the-box. You can easily replace the old assets pipeline using Turbolink with Webpack when start a new project:


```
rails new myapp --skip-coffee --skip-turbolinks --skip-sprockets --webpack
```

(or `--webpack=vue` if you want to use VueJS along with Webpack)

# Rails in Docker

In these days, people use Docker to quickly create a development environment. You can easily follow [this guide](https://docs.docker.com/compose/rails/) to create a Rails environment in your machine.

# Rails with Webpack in Docker

If you want to setup Rails enviroment in Docker, and use Webpack, there need to be some changes from the guide.

* First, you need to install the latest Node version. The Dockerfile they give you in the guide does include installing Nodejs, but it will be the v4, which is quite outdated. You can check the latest version [here](https://github.com/nodesource/distributions/blob/master/README.md#debinstall). So, to install v11, I added these 2 lines into Dockerfile:
```docker
RUN curl -sL https://deb.nodesource.com/setup_11.x | bash -
RUN apt-get install -y nodejs
```
* Then, you need to install Yarn so that the command `rails  webpacker:install` will work automatically at the time you run `rails new`. Of course, you can install Yarn later and run the webpacker install command manually. The installation guide is [here](https://yarnpkg.com/lang/en/docs/install/#debian-stable). Based on that, I added following lines to Dockerfile.
```docker
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list
RUN apt-get update && apt-get install yarn
```

OK, so basically you only need 2 changes in order to make Docker work with Rails and Webpack. This is my final Dockerfile:
```docker
FROM ruby:2.6
RUN apt-get update -qq && apt-get install -y postgresql-client
RUN curl -sL https://deb.nodesource.com/setup_11.x | bash -
RUN apt-get install -y nodejs
RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list
RUN apt-get update && apt-get install yarn
RUN mkdir /my_app
WORKDIR /my_app
COPY Gemfile /my_app/Gemfile
COPY Gemfile.lock /my_app/Gemfile.lock
RUN bundle install
COPY . /my_app

COPY entrypoint.sh /usr/bin/
RUN chmod +x /usr/bin/entrypoint.sh
ENTRYPOINT ["entrypoint.sh"]
EXPOSE 3000

CMD ["rails", "server", "-b", "0.0.0.0"]
```

