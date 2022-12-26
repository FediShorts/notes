# Notes
All the notes for the project will be sorted in this repository, keeping them all central!

- [Notes](#notes)
- [**Config Server**](#config-server)
  - [What is it for?](#what-is-it-for)
  - [How do I use it?](#how-do-i-use-it)
  - [How is it secured?](#how-is-it-secured)
    - [Backend](#backend)
    - [Frontend](#frontend)

# **Config Server**

- Language: NodeJS

## What is it for?

Upon starting any of the micro services, they will make a request to this config server in order to get the configuration for that specific service. This is done to keep the configuration for the services in one place, and to make it easier to change the configuration for all the services at once as opposed to having to go through each config.json or .env file per service.

## How do I use it?

It will have a Frontend and a Backend. The Admin / Dev would interact with the Frontend to change the configuration for each of the services such as the Authentication service. The Frontend will then make a request to the Backend to update the configuration for the services. Upon doing this, you can choose to either restart the services manually, or have the Backend restart the services for you. The Backend will then make a request to your specificied service in order to restart it.

## How is it secured?

### Backend

When any of the Micro Services make a request to the config server, the request will contain a 'token'. If the token matches the token found in the database, then the server will return the correct configuration. If it is not, then the server will return a 401 Unauthorized error.

Could also verify IP address of the request, and only allow requests from the IP address of the service- could be very useful for double security.

It will be the same when the Config Server makes a request to any of the Micro Services. It will make a request with a token which was defined previously when the server last had it's config updated. This is used to ensure that nobody random can make these privileged requests. It will also ensure that the request is coming from the configurations server's IP address.

### Frontend

Simple authentication which will rely on the Authentication microservice. If the user enters a correct username and password (Maybe could add 2FA?), the Authentication service will return a token which will be stored in the user's cookies. Upon making a request to the Config Server e.g. to list configurable servers, the token will be sent in the request. If the token is ever invalid, it will automatically redirect users back to the login page.
