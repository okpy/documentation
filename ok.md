[Ok Documentation](https://okpy.github.io/documentation)
=====

OK helps instructors run better programming projects by running tests, tracking student progress, and enabling automatic grading.

> <img src="https://okpy.github.io/documentation/logo.svg" height="100px" />

Contact us to use our hosted service (http://okpy.org) for your course.

The ok.py software was developed for [CS 61A at UC Berkeley](http://cs61a.org), a large in person introductory programming course (~800 to 1500 students/semester). Ok is currently used by multiple computer science courses.

> <br>
> [View CS 61A Course Material >](http://cs61a.org)

# Ok Software Suite
The OK software suite is comprised of three parts: a server, a client, and the autograder.
> * Server: Hosted Web Application to store submissions
> * Client: Enables students to run tests locally and submit
> * Autograding: Secure grading of student code.

If you are an instructor you may use any (or all) of the three parts for your programming project.
In combination, the software suite makes running projects

## Use OK in your course

If you are a computer science educator interested in evaluating OK for use in a course - please reach out to us.

You may either self host the server on your school infrastructure or use the hosted version of OK.

We'd be happy to help you decide if OK is a good fit. You can email us at ok@cs61a.org or by clicking the "contact us" link.

> [Contact Us! >](mailto://ok@cs61a.org?subject=Using%20OK%20in%20a%20course&cc=denero@berkeley.edu)

##  Server
The [OK Server](okpy) collects student submissions, displays students submissions, and provides a dashboard for instructors to control their assignments.
> [GitHub Repo >][ok-server-github]

> [API Docs >][ok-api-documentation] [Developer Docs >][ok-server-documentation]

[![Build Status](https://travis-ci.org/Cal-CS-61A-Staff/ok.svg)](https://travis-ci.org/Cal-CS-61A-Staff/ok) [![Coverage Status](https://coveralls.io/repos/github/Cal-CS-61A-Staff/ok/badge.svg?branch=master)](https://coveralls.io/github/Cal-CS-61A-Staff/ok?branch=master)

[View Okpy.org >][okpy]

[okpy]: http://okpy.org
[ok-server-documentation]: ok-server.html
[ok-api-documentation]: ok-api.html
[ok-server-github]: https://github.com/Cal-CS-61A-Staff/ok

## Client
The [OK Client](https://github.com/Cal-CS-61A-Staff/ok-client) allows students to check their solution against tests locally, answer free response questions, and submit to OK Server.

[![Build Status](https://travis-ci.org/Cal-CS-61A-Staff/ok-client.svg?branch=master)](https://travis-ci.org/Cal-CS-61A-Staff/ok-client)

> [Github Repo >](https://github.com/Cal-CS-61A-Staff/ok-client)

> [Documentation >](https://github.com/Cal-CS-61A-Staff/ok-client/wiki)

> [Assignment Setup Info >](client.html)

The OK Client supports a variety of languages, including [Python 3](python.org), SQL, Logic (Prolog dialect), Scheme - and more languages can be added.

It currently requires Python 3 to be installed on the students computer. The client can be run without the use of the server if desired.

You can view the usage of OK Client in a real project [Yelp Maps](http://nifty.stanford.edu/2016/hou-zhang-denero-yelp-maps/) (presented at [Nifty Assignments SIGCSE 2016](http://nifty.stanford.edu))
```bash
# Example Assignment Demo
$ unzip maps.zip; cd maps
$ python3 ok -q 00 -u
$ python3 ok -q 00
```

[Example Assignment >](http://nifty.stanford.edu/2016/hou-zhang-denero-yelp-maps/maps_deploy.zip)

## Server Side Autograder
The Server Side Autograder executes your grading code in a sandboxed container on a remote server.

The autograder supports <b>any language or grading setup</b> that can run in a virtual machine.

> [API Documentation >][autograder-docs]

* Provide feedback to students without distributing the contents of the tests with your project.
* Quickly grade assignments in OK Server
* Grade code not submitted through OK through the API

The autograder provides optional email notification and a cooldown period for autograder results.
> [View Examples >][autograder-docs]

You can request access by sending an email to sumukh@berkeley.edu with details about your use case.

[Request Access >](mailto://sumukh@berkeley.edu?subject=OK%20Autograder)

[autograder-docs]: autograder.html


