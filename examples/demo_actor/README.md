# DemoActor

This document describes how to create an Actor(DemoActor) and invoke its methods on the client application.

* **The actor interface(demo_actor_interface.py).** This contains the interface definition for the actor. Actor interfaces can be defined with any name. The interface defines the actor contract that is shared by the actor implementation and the clients calling the actor. Because client may depend on it, it typically makes sense to define it in an assembly that is separate from the actor implementation.

* **The actor service(demo_actor_service.py).** This implements Flask web service that is going to host the actor. It contains the implementation of the actor, `demo_actor.py`. An actor implementation is a class that derives from the base type `Actor` and implements the interfaces defined in `demo_actor_interface.py`.

* **The actor client(demo_actor_client.py)** This contains the implementation of the actor client which calls DemoActor's method defined in Actor Interfaces.

## Prerequisites

* [Install Dapr standalone mode](https://github.com/dapr/cli#install-dapr-on-your-local-machine-standalone)
* [Install Python 3.8+](https://www.python.org/downloads/)

## Install Dapr SDK

You can install dapr SDK package using pip command:

```sh
pip3 install dapr-dev
```

Or, you can use the current repo:

```sh
cd <repo root>
pip3 install -r ./dev-requirement.txt
export PYTHONPATH=`pwd`
```

## Try DemoActor example

1. Run Demo Actor service in new terminal window

```bash
$ cd examples/demo_actor
$ dapr run --app-id demo-actor --app-port 3000 python3 demo_actor_service.py
...
== APP ==  * Serving Flask app "DemoActorService" (lazy loading)
== APP ==  * Environment: production
== APP ==    WARNING: This is a development server. Do not use it in a production deployment.
== APP ==    Use a production WSGI server instead.
== APP ==  * Debug mode: off
== APP ==  * Running on http://127.0.0.1:3000/ (Press CTRL+C to quit)
== DAPR == time="2020-02-29T13:52:15-08:00" level=info msg="application discovered on port 3000"
== APP == 127.0.0.1 - - [29/Feb/2020 13:52:15] "GET /dapr/config HTTP/1.1" 200 -
...
== APP == Activate DemoActor actor!
== APP == 127.0.0.1 - - [01/Mar/2020 18:50:27] "POST /actors/DemoActor/1 HTTP/1.1" 200 -
== APP == 127.0.0.1 - - [01/Mar/2020 18:50:27] "PUT /actors/DemoActor/1/method/GetMyData HTTP/1.1" 200 -
== APP == 127.0.0.1 - - [01/Mar/2020 18:50:27] "PUT /actors/DemoActor/1/method/GetMyData HTTP/1.1" 200 -
== APP == 127.0.0.1 - - [01/Mar/2020 18:50:27] "PUT /actors/DemoActor/1/method/SetMyData HTTP/1.1" 200 -
== APP == 127.0.0.1 - - [01/Mar/2020 18:50:27] "PUT /actors/DemoActor/1/method/GetMyData HTTP/1.1" 200 -
...
```

2. Run Demo client in new terminal window

```bash
$ cd examples/demo_actor
# Run actor client
$ dapr run --app-id demo-client python3 demo_actor_client.py
...
== APP == b'{"data":"default","ts":"2020-03-02T02:50:27.381Z"}'
== APP == {'data': 'default', 'ts': datetime.datetime(2020, 3, 2, 2, 50, 27, 386000, tzinfo=tzutc())}
== APP == {'data': 'new_data', 'ts': datetime.datetime(2020, 3, 2, 2, 50, 27, 395000, tzinfo=tzutc())}
...
```