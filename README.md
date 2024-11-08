# Big Brother

## Introduction 

This project main goal is to automatically add to library, albums that are more than X% liked. This work only for [Spotify](https://spotify.com).

The project is represented by 3 Github repositories: this one, [bigbrother-server](https://github.com/felixmielcarek/bigbrother-server), and [bigbrother-script](https://github.com/felixmielcarek/bigbrother-script). Their roles and interactions are going to be explained in this document.

## How does it work ?

There are 2 different parts to consider: the activation of the service and the running of the script which will update your library each day.

### Activation of the service

The goals of this first step are:
* to allow our users to activate our service on their Spotify account.
* to store what we will need to interact with their Spotify library.

To achieve these goals, a few things has to be setup:
1. Develop a interface to interact with our users.
2. Identify and set up the correct Spotify authentification (the [Web API](https://developer.spotify.com/documentation/web-api) is used in this project).
3. Set up and store in a database our needed informations provided by authentification.

#### 1. The interface

The interface is a website, the code is in this repository.

The main page has a button which ables to start the Spotify authentification.

#### 2. Spotify authentification

The best suited way for this to project to authentificate is the [Authorization Code Flow](https://developer.spotify.com/documentation/web-api/tutorials/code-flow). Here is the diagram from the documentation which explains the steps:

![Authorization Code Flow diagram](https://developer.spotify.com/images/documentation/web-api/auth-code-flow.png)

Lets explain which part of this project interacts in each step:
* Step 1: the activation button from the main page (`/`) of the `website` initialize the **request authorization to access data**.
* Step 2: after the user logged in, informations given by Spotify are recovered by the callback endpoint of the `server part`. This endpoints triggers the **request access and refresh token**.
* Step 3 and 4: tokens are also recovered by the `server part`. Requests to Web API are done by the `script part` of this project.

#### 3. Store the authentification informations

The `server part` of the project triggers tokens recovery. When this is done, it also recover the Spotify account id of the user, it then stores these informations into a [Firebase](https://firebase.google.com/) database.

### Script description

The script algorithm is described in its [repository](https://github.com/felixmielcarek/bigbrother-script).

## Deployment