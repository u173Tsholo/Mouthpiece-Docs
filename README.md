# Mouthpiece RESTful API
### The API interface that will be used to facilitate inter-module communication.

Teams developing a server-side module will be required to write code supporting a completely independent web-host instance that will communicate with other server modules via HTTP (on a closed virtual network). For example, a team might choose to run an Apache server with API logic written in PHP, or a Django/Flask server with Python code. Whatever your choice, you will be expected to run your repository as a representation of the primary script directory that will be cloned into Integration's installed runtime of the desired server technology. For convenience's sake, please keep to a popular system such as Apache, Django, or even Spring Boot for brave folk. The more obscure your technology, the more likely it is that we will not be able to configure it in due time.
Please see [README-API.md](https://github.com/COS301-Theta/Mouthpiece-Docs/blob/master/README-API.md) for specification details.

# Mouthpiece App Interfaces
### The Java class interfaces that will must be expanded upon by each module within the mobile app.

Due to the more closely-tied nature of mobile app classes, individual teams will all be provided with a fork of the central Mouthpiece-App repository. These teams should be familiar with [DartLang](https://dart.dev/) and [dependency management within the Flutter Framework using Pub and .yaml](https://flutter.io/using-packages/) (no knowledge of the rest of the framework is required!) Please note that installing Flutter can be a time-consuming process and should be started sooner rather than later.
Please see [README-API.md](https://github.com/COS301-Theta/Mouthpiece-Docs/blob/master/README-APP.md) for specification details.

# Queries
Any queries about the above documentation must please be added to the [issues tab](https://github.com/COS301-Theta/Mouthpiece-Docs/issues) above so that Integration may address them as quickly as possible.