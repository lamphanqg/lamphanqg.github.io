---
layout: post
title: "Run Capybara test for Rails in Docker"
tag: [ruby, rails, docker, capybara, TIL]
---
## It's not that easy for me

This took me half a day to make it work. I thought it would be easy because in this containerized era, a Rails developer should normally make Rails run in a container, and it's not convenient to do all your feature tests with a headless browser inside a container, and without any GUI.

In fact, there are some guides in Japanese and English, but somehow most of them did not work out of the box, so I had to do some trial and error.

## Result

Here is what works for me.

### docker-compose.yml


```yaml
version: '3'
services:
  db:
    image: postgres
    volumes:
      - ./tmp/db:/var/lib/postgresql/data
    networks:
      rails-network:
        aliases:
          - db.com
  web:
    build: .
    volumes:
      - .:/my_app
    ports:
      - "3000:3000"
    depends_on:
      - db
    networks:
      rails-network:
        aliases:
          - web.com
  chrome:
    image: selenium/standalone-chrome-debug:3.9.1-actinium
    ports:
      - "4444:4444"
      - "5900:5900"
    depends_on:
      - web
    networks:
      rails-network:
        aliases:
          - chrome.com
networks:
  rails-network:
```

### Dockerfile

```docker
FROM ruby:2.6
# libnss3-dev is necessary to install google-chrome & run chromedriver-helper
RUN apt-get update -qq && apt-get install -y postgresql-client libnss3-dev
# Install the newest version of NodeJS
RUN curl -sL https://deb.nodesource.com/setup_11.x | bash -
RUN apt-get install -y nodejs
# Install google-chrome for debian
RUN wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
RUN dpkg -i google-chrome*.deb || apt update && apt-get install -f -y

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

### Gemfile

```ruby
# frozen_string_literal: true

source "https://rubygems.org"
git_source(:github) { |repo| "https://github.com/#{repo}.git" }

ruby "2.6.0"

# Bundle edge Rails instead: gem 'rails', github: 'rails/rails'
gem "rails", "~> 5.2.2"

...

group :development, :test do
  # Call 'byebug' anywhere in the code to stop execution and get a debugger console
  gem "byebug", platforms: [:mri, :mingw, :x64_mingw]
  gem "rspec-rails"
end

group :test do
  gem "capybara"
  gem "selenium-webdriver"
  gem "chromedriver-helper"
end

...


```

### spec/rails_helper.rb

```ruby
# frozen_string_literal: true

# This file is copied to spec/ when you run 'rails generate rspec:install'
require "spec_helper"
ENV["RAILS_ENV"] ||= "test"
require File.expand_path("../../config/environment", __FILE__)
# Prevent database truncation if the environment is production
abort("The Rails environment is running in production mode!") if Rails.env.production?
require "rspec/rails"
# Add additional requires below this line. Rails is not loaded until this point!
require "capybara/rspec"
require "selenium/webdriver"

# ...

Dir[Rails.root.join('spec', 'support', '**', '*.rb')].each { |f| require f }

# ...

```

### spec/support/capybara.rb

```ruby
if ENV["LAUNCH_BROWSER"]
  # To test with browser opened in VNC screen sharing window
  Capybara.configure do |config|
    config.server_host = "web.com"
    config.javascript_driver = :selenium_chrome
  end

  Capybara.register_driver :selenium_chrome do |app|
    Capybara::Selenium::Driver.new(
      app,
      browser: :remote,
      desired_capabilities: Selenium::WebDriver::Remote::Capabilities.chrome(
        chromeOptions: {
          args: [
            "window-size=1024,768"
          ]
        }
      ),
      url: "http://chrome:4444/wd/hub"
    )
  end
else
  # To test with headless browser inside web container
  Capybara.server = :puma, { Silent: true }

  Capybara.register_driver :chrome_headless do |app|
    options = ::Selenium::WebDriver::Chrome::Options.new

    options.add_argument('--headless')
    options.add_argument('--no-sandbox')
    options.add_argument('--disable-dev-shm-usage')
    options.add_argument('--window-size=1400,1400')

    Capybara::Selenium::Driver.new(app, browser: :chrome, options: options)
  end

  Capybara.javascript_driver = :chrome_headless
end
```

## How to run tests

* With headless Chrome inside web container:
```shell
docker-compose exec web rspec
```
* With GUI Chrome browser inside chrome container:
```shell
open vnc://localhost:5900 # To open screen sharing window. Input "secret" if asked for password.
docker-compose exec -e LAUNCH_BROWSER=true web rspec
```

FYI, when you run rspec test without any specified file or folder, it will run all tests inside spec folder, but Capybara is only called when it comes to feature tests (maybe system tests too, in Rails > 5.1, but I haven't checked it).
