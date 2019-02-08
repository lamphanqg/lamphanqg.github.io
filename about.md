---
layout: page
title: About Me
permalink: /about/
---

![My photo](/assets/images/about_image.jpg)

(I'm on the left)

I'm a SE/BrSE from Vietnam living near Tokyo. My experience is mainly about coding in PHP and Ruby, as well as connecting Vietnamese and Japanese team members.

## Education

I graduated from Business Management department of Ritsumeikan Asia Pacific University. My major was Accounting, but I jumped to programming jobs right after 3 weeks of probation period for an audit position in PwC Vietnam (It was too formal, and too much extrovert-oriented for me).

My programming skills are learnt partly from my Pascal programming course in highschool, and 4 programming courses with VB.net in college. After graduation, I'm continuing to learn proramming and PM skills from MOOC courses (Coursera is my favorite), articles, books, and recently from a Vietnamese programmer community.

## Work Experience

### 1st company

My projects:

* PSD to HTML converting
* Gmail SSO implementation for Wordpress page
* Create forms in raw PHP
* Create simple iPad app in Obj-C
* Create member introduction site with FuelPHP

This is a small company whose main business is mainly creating homepages and campaign sites/apps. My technical team has 2 seniors and 3 juniors (including me). Projects are small, so usually each one of member will take care of 2-3 projects at the same time, and have a team meeting every week to report the progress. Tooling in the company was quite poor. Even though it was 2015, our seniors did not know about version control, let alone Git. Codes were zipped and named with date and time, then uploaded to server through a FTP client. Another junior and I brought Git to the company.

Here I learnt about basic phases in producing websites: design, HTML-CSS-JS coding, PHP (I only know PHP at the time) integrating, unit test, deploying.

### 2nd company (current)

#### Project 1

It's a B2B system that support companies to manage recruitment posts, applicants, screening schedules, etc. It's written in Rails 2, and our team joined in the maintenance phase. I have to say, this project is old, and lots of code are not that good, but it taught me a lot about Rails, AWS, MVC, Docker, etc. The technology stack of it mainly includes Ruby on Rails, Passenger, Apache, MySQL. There are 2 applications. One for managing side, which let companies' recruiters manage data related to recruiting jobs and applicants, and another one to show recruiting posts to public. We have a batch to sync data between those applications regularly. In the former application, we also have some crawlers that get datas from recruiting sites, with our client's username and password provided. They may be the most tricky part in the application, because they're built dynamically. When we want to add more recruiting sites to crawl, we only need to add some specific commands with parameters into database, and the crawler will read and execute those commands. There's a bit metaprogramming here, and I think that's one of the reasons they chose Ruby for this.

In this project, I have been playing as a BrSE, standing between the client's DevOps team in Japan, and my engineer team in Vietnam. In Japan side, we try to make it following agile principles. We have daily scrums, weekly planing meetings and retrospectives. We also have on-the-fly technical meetings, where we discuss and decide, to some extent, the structure and logic of new functions, or cause and solution of bugs. Then, everyday, I make Jira tasks and translate everything into Vietnamese, and manage my team to fulfill all requirements we decided. Nevertheless, because I know best about requirements, and (unfortunately) have the most experience with the project as well as Rails framework, I usually have times when it's better, faster, simpler for me to write the code myself. To be honest, I enjoy those times a lot more than management, planning, or reporting times.

#### Project 2

I will write about it later.

## What I want

Currently, I would like to have a job where I can practice and enhance my programming skills. Recently I'm looking into some following topics:

* TDD in Rails with Rspec
* Vuejs
* Typescript
* Elixir
* EC
* Chat application
* Agriculture & gardening (I have a small garden near my house, where I enjoy growing my veggies)
* Data analysis and visualization (like in business intelligence systems)
* Design system (in order to make peace between designers and developers)

## What I don't want

* Too much management stuff (Basically it's what I'm doing, and I think I am in the best place for this kind of job)
* Pulic speaking (I have a strong speech anxiety, even in my native language, and even through a phone)

## Tech stack

### Have experience, able to use quickly with support of google search

* Ruby
  * Ruby on Rails
  * Capybara
* Docker (mainly docker-compose)
* Vagrant
* Javascript (mostly jquery)
* PHP
  * Laravel
* git
* shell script
* Apache
* Passenger

### Tried some times, easy to read, may take time to use

* PHP
  * FuelPHP
  * CodeIgniter
* Python
* Octave (tried it when I take a course in machine learning)
* Vue (basic use only)
* Swift
* nginx
* C# (used when I try to learn game programming with Unity)

### Have interest in, but not have enough time to write anything

* Vue as a framework to build SPA
* Elixir

### Don't like much, but can learn if work requires

* React
* Go
* Java

## Language ability

### English

I have learned it for 15 years, got IELTS 6.5 in 2010 in order to come to my college. It's the main language used in all my college classes, so I think I have no problem using English in business enviroment.

### Japanese

I started Japanese from college, and mostly learned it in part-time jobs, daily life problem solving, and my current job. I've obtained N1 in 2017.
