# electric-arc-standard
A software standard proposal for operators to share their charge points.

---

# Why

A standard should empower operators to share their charge points without burdening them at the same time.

Getting started should be as easy as sharing a file and available features is at the operators discretion.
Faults should be tolerated to alleviate the operator downtime.
Clients must carry a load, so scaling the number of clients doesn't come at the expense of the operator.

# How

I propose 2 services:

1. List of all charge points
2. Details on a charge point

Why two services you ask? I would like to split them up into static information and dynamic information. This is so the client can get an overview and then hammer select charge point it's interested in.

By splitting the services into static and dynamic information, I can make sure that the biggst file (the list of all charge points) arent transmitted very often.

### Thought example: The app
Lets say the client is an app. The user opens the app, which fetches the list of all charge points
and quickly displays them for the user, all charge points statuses are set to unknown. 

The app determines which charge points are included in the users current view and fetches dynamic information for these and updates them on for the user (sets them to available, occupied, unknown or broken)

The user selects an available charge point to navigate to and the app frequently asks for the details of that specific charge point and provides last modified. Most of the time, the operator returns a "nothing's changed"-response without further data, but at some point a new status is received: Occupied! The app warns the user.

The user wants to find alternative options, so he goes back to the overview. The app asks for a list of all charge points and supplies last modified. The operator returns a "nothing's changed"-response and the app uses the list from the cache.

The caching mechanism described are standard HTTP behavior, so it could be implemented simply by placing some xml files on a webserver. Standard proxy servers can add fault tolerance and alleviate server pressure. It's possible to handle a growing amount of clients, since the client does the heavy lifting like filtering and sorting.

## 1. List of all charge points

This list contains all charge points along with information which is not likely to change.

Information on a charge point that is not likely to change could be:
* Unique ID
* Name
* Address
* Type
* GPS Coordinates
* Optional link to dynamic information
* ect.

## 2. Details on a charge point

This contains details for a specific charge point that are likely to change. The client will only find this if a link was provided.

Information on a charge point that is likely to change could be:
* Status
* Error message
