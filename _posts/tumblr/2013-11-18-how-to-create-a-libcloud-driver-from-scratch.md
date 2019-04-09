---
layout: post
title: 'How to create a libcloud driver from scratch: the NephoScale case'
date: '2013-11-18T16:19:00+02:00'
tags:
- open source
- python
- libcloud
tumblr_url: https://blog.mist.io/post/67366170830/how-to-create-a-libcloud-driver-from-scratch
---
The use of Web APIs for communicating with cloud providers has been increasing over the last few years and every computing and web services provider is expected to maintain a well documented API for their clients. It enables developers, system administrators and engineers to &nbsp;automate tasks and assists in the deployment of servers and services; and as multi-cloud setups become the norm and cloud management tools a necessity, a well documented API significantly enhances the user experience.

As one would expect, there are many tools in different programming languages that facilitate the communication with each cloud API.&nbsp; On the Python world, [libcloud](http://libcloud.apache.org "Libcloud") is the de facto library to communicate with one or more cloud providers. It abstracts away differences among multiple cloud provider APIs and provides a unified way to manage cloud resources.

Mist.io makes heavy use of libcloud in order to support a wide range of providers in a unified interface. [**You can try it out for yourselves by logging in mist.io.**](https://mist.io/ "mist.io homepage")

![image](/assets/tumblr-images/tumblr_inline_mwmhax11ie1rgqrs8.png)

Libcloud was initially developed by the awesome **CloudKick** team. Today it is an Apache foundation project. It has a healthy community, with lots of contributors, [updated documentation](https://ci.apache.org/projects/libcloud/docs/), mailing lists, a [ticketing system](https://issues.apache.org/jira/browse/LIBCLOUD), a continuous integration platform and an established [user base](http://libcloud.apache.org/whos-using.html "who's using libcloud").

Libcloud allows users to manage four different cloud resources: Cloud Servers, Cloud Storage, Load Balancers as a Service and DNS as a Service. Cloud Servers is the oldest and more mature part of the library and currently supports more than 26 providers.

Many cloud providers eg Amazon, Rackspace, Linode, DigitalOcean etc, are already supported by libcloud. In this post we want to illustrate how easy it is to add support for new providers, by documenting the steps we followed to add support for a cutting edge US-based cloud provider [NephoScale.](http://nephoscale.com "NephoScale")

![image](/assets/tumblr-images/tumblr_inline_mwmhoc1JAy1rgqrs8.png)

**What should we implement on our compute driver**

Our goal is to create a compute driver that will provide the most common sever management actions. Actions that are universally supported by libcloud for all the cloud providers:

- list\_nodes: provide a list with our nodes, including information such as public/private ip addresses, node state -running, rebooting, stopped etc- and some extra metadata such as the image, location etc
- list\_images: list all the images the provider supports for faster node creation
- list\_sizes: list different plans, providing data such as CPU, ram and disk size. Some providers include the price of each size plan
- list\_locations: Most cloud providers allow us to specify the different zone where we can deploy nodes

and the following node actions:

- create\_node: create a node specifying it’s size, image, name and location
- deploy\_node: create a node and deploy a public ssh key for server access, and optionally run a deploy script after the node is initialized and can be accessible
- reboot\_node/shut down\_node
- start\_node/stop\_node: start a stopped node, or stop a started one
- destroy\_node: Delete the node

We will implement the above for the NephoScale driver, plus some extra functions that will allow us to create, list and delete keypairs. Keypairs in NephoScale can be ssh or password keys, for server or console access. Digital Ocean also allows the listing/creation/deletion of keys through it’s API, while other cloud providers do not offer that functionality. Other examples of provider specific functionality include tagging, where only a few cloud providers (eg AWS, Rackspace) provide. In this example and in order to be as generic as possible, we will target functionality that is available for most -if not all- cloud providers.

**Getting started with the compute driver**

We’ll need access to our provider API. In this scenario, the [Nephoscale API](http://docs.nephoscale.com/#!/reference/nephos).

We will use the Digital Ocean driver as a template as it is one of the most recent, well written and up-to-date drivers. There is an effort by libcloud to standardize the basic functionality for all the compute drivers, and update the oldest ones. It is also very important when cloud providers contribute code or review the drivers created for them in libcloud, or even better maintain them as their API changes.&nbsp;

Compute drivers are located on libcloud/compute/drivers and they all inherit from the base compute driver NodeDriver on libcloud/compute/base.py. The driver is well documented and we can learn a lot from reading the comments and of course the code.

**Add the driver on types.py and providers.py**

We’ll start by adding our provider on the DRIVERS dict on libcloud/compute/providers.py and on Provider class on libcloud/compute/types.py. It’s pretty straightforward as we can see from the existing drivers.

**Create driver file and start writing the code**  
On libcloud/compute/drivers/ we create a file with the name of our provider: nephoscale.py in our case.

After the initial imports that we copy from the Digital Ocean driver, we specify the API endpoint of our cloud provider

    API\_HOST = 'api.nephoscale.com'

and a dict with the states that the nodes can have. This differs from provider to provider, with some providers having just a few states -running, stopped - and others having intermediates ones -e.g. rebooting, shutting down etc.

    NODE\_STATE\_MAP = {
    &nbsp;&nbsp;&nbsp; 'on': NodeState.RUNNING,
    &nbsp;&nbsp;&nbsp; 'off': NodeState.UNKNOWN,
    &nbsp;&nbsp;&nbsp; 'unknown': NodeState.UNKNOWN,
    }

**Writing the NephoscaleNodeDriver**

We then need to create a NodeDriver based driver

    class NephoscaleNodeDriver(NodeDriver):
    &nbsp;&nbsp;&nbsp; """Nephoscale node driver class"""
    
    &nbsp;&nbsp;&nbsp; type = Provider.NEPHOSCALE
    &nbsp;&nbsp;&nbsp; api\_name = 'nephoscale'
    &nbsp;&nbsp;&nbsp; name = 'NephoScale'
    &nbsp;&nbsp;&nbsp; website = 'http://www.nephoscale.com'
    &nbsp;&nbsp;&nbsp; connectionCls = NephoscaleConnection
    &nbsp;&nbsp;&nbsp; features = {'create\_node': ['ssh\_key']}
    
     def list\_locations(self):
     ...

Here we specify the type (same as on libcloud/compute/types.py), the connection class (NephoscaleConnection) and a features dict with the way deploy\_node will try to authenticate to the created node after create\_node has run . Inside the NephoscaleNodeDriver is where all our functions will live. We’ll write all functions we want to implement there, list\_images, list\_nodes, reboot\_node etc. To keep consistency, we make sure that list\_nodes returns a list of Node objects, list\_images a list of NodeImage objects, list\_sizes a list of NodeSize objects and list\_locations a list of NodeLocation objects.

**Writing the Connection class**

NephoscaleConnection is the Connection class that will handle connection, authentication and send the request to the NephoScale API endpoint. All drivers implement a Connection class that is responsible for sending the request data, adding HTTP headers or params and also encoding the request body.The API host endpoint is specified there, along with the HTTP headers or params that are used to authenticate for each provider.

NephoScale API calls require HTTP Basic Authentication with the user/password base64 encoded on every request, so we’ll add it to add\_default\_params.&nbsp;

    class NephoscaleConnection(ConnectionUserAndKey):
    &nbsp;&nbsp;&nbsp; """Nephoscale connection class
    
    &nbsp;&nbsp;&nbsp; Authenticates to the API through Basic Authentication
    &nbsp;&nbsp;&nbsp; with username/password
    
    &nbsp;&nbsp;&nbsp; """
    &nbsp;&nbsp;&nbsp; host = API\_HOST
    &nbsp;&nbsp;&nbsp; responseCls = NephoscaleResponse
    
    &nbsp;&nbsp;&nbsp; def add\_default\_headers(self, headers):&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; user\_b64 = base64.b64encode(b('%s:%s' % (self.user\_id, self.key)))
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; headers['Authorization'] = 'Basic %s' % (user\_b64.decode('utf-8'))
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; return headers
    

Other providers handle authentication with requiring the params on each request -API key, password, secret etc.

DigitalOcean for example requests the client ID and API key on each request, and we pass it with add\_default\_params

    class DigitalOceanConnection(ConnectionUserAndKey):
    &nbsp;&nbsp;&nbsp; ...
    
    &nbsp;&nbsp;&nbsp; def add\_default\_params(self, params):
     params['client\_id'] = self.user\_id
     params['api\_key'] = self.key
     return params

Inside our Connection class we can override encode\_data method, so that it encodes data as requested. For example we can set it to encode data with json.encode, if the API endpoint expects json encoded data.&nbsp;

The Connection class that gets overridden on our driver lives in libcloud/common/base.py.&nbsp;

**Writing the Response class**

All drivers also need a response class that handles responses of the API endpoint, parses body and returns Exceptions or the actual return content.&nbsp; Response classes for the libcloud drivers derive from JsonResponse or XMLRPCResponse, depending on the response type of the cloud provider -Json or XML. Both derive from Response class that lives in libcloud/common/base.py. We want to make sure here that in the case of errors we throw the correct exceptions - e.g. InvalidCredsError for failed authentication.

    class NephoscaleResponse(JsonResponse):
    &nbsp;&nbsp;&nbsp; """Nephoscale API Response"""
    
    &nbsp;&nbsp;&nbsp; def parse\_error(self):
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; if self.status == httplib.UNAUTHORIZED:
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; raise InvalidCredsError('Authorization Failed')
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; if self.status == httplib.NOT\_FOUND:
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; raise Exception("The resource you are looking for is not found.")
    
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; return self.body
    

**Notes on create\_node and deploy\_node**

Most of the drivers of libcloud implement create\_node to return a Node object, of the newly created host. Function deploy\_node is not overridden , but instead gets called from the NodeDriver, since it does a lot of things that need not be rewritten, such as checks that the node public ip is up, that the machine can be accessible, that it can authenticate via password or ssh key, and optionally runs a deploy script.

The default behaviour of create\_node is to send the request for the node to be created, and get a response with the created node id. Most cloud providers we have used reply with the ID of the created node, along with information about the state of the node, public ip etc. NephoScale does not respond with an ID once it receives the request for a node to be created, but instead replies with a status ID that is not very helpful. So as soon as we send the request and receive a valid response, we’ll be waiting for a bit and try to get the machine information by requesting list\_nodes and checking if the machine has appeared. When we have the machine Node, we return create\_node. In this way the behaviour of create\_node stays consistent with the other drivers and deploy\_node works as expected, out of the box.

Once again, reading the comments of&nbsp;NodeDriver on libcloud/compute/base.py for deploy\_node and create\_node functions will help us implement create\_node and make sure deploy\_node will work.

**Keep consistency, hard code things when necessary**

One of the cool things with libcloud is that it standardizes things. It enforces the use of basic entities -nodes, sizes, images, locations- for all cloud providers it supports.

[SoftLayer](http://softlayer.com) for example does not provide a size entity through it’s API, but rather expects that we specify CPU, RAM and disk size when we create a node. The way libcloud driver is implemented, one can either pass these manually, or simply select one of the sizes provided by list\_sizes.

Ideally all size/image/location related data should be fetched by asking the provider. When this is not possible, for example when the provider does not implement a request, we need to hard code these settings. For example the pricing info for some of the providers isn’t returned while asking for the sizes (list\_sizes) and thus is hard coded on libcloud/data/pricing.json. Or the info regarding sizes for AWS is not available through an API call. The obvious disadvantage with hard coding things is that they have to be maintained to reflect the current status of the provider’s API.

**Working with our driver**

While writing the driver, it is always good to test it:

    user@user:~/dev/libcloud$ python
    \>\>\> from libcloud.compute.types import Provider
    \>\>\> from libcloud.compute.providers import get\_driver
    \>\>\> driver = get\_driver(Provider.NEPHOSCALE)
    \>\>\> conn = driver('user','correct\_password')
    \>\>\> conn.list\_nodes()
    [\<Node: uuid=e20bdbf7ef6890645f5b217e0bd2b5912b969cc1, 
    name=nepho-7, state=0, public\_ips=['198.89.109.116'], 
    provider=NephoScale ...\>]

We can use the connection class for GET or POST requests according to the API, for testing and debugging issues. E.g to implement the list\_locations for NephoScale, we need to see what the response is after requesting the locations (via [https://api.nephoscale.com/datacenter/zone/](https://api.nephoscale.com/datacenter/zone/))

    \>\>\> conn.connection.request('/datacenter/zone/').object
    {'success': True,
    'total\_count': 2, 
    'subcode': 0,
    'message': 'Your request was processed successfully.',
    'data': [
    &nbsp; {'datacenter': {
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 'id': 1,
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 'airport\_code': 'SJC', 
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 'name': 'SJC-1',
    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; 'uri': 'https://api.nephoscale.com/datacenter/1/'},
    &nbsp;&nbsp; 'uri': 'https://api.nephoscale.com/datacenter/zone/86945/',
    &nbsp;&nbsp; 'name': 'SJC-1',
    &nbsp;&nbsp; 'id': 86945},
    &nbsp; {'datacenter': {
    &nbsp;&nbsp;&nbsp;&nbsp; 'id': 3, 
    &nbsp;&nbsp;&nbsp;&nbsp; 'airport\_code': 'RIC', 
    &nbsp;&nbsp;&nbsp;&nbsp; 'name': 'RIC-1',
    &nbsp;&nbsp;&nbsp;&nbsp; 'uri': 'https://api.nephoscale.com/datacenter/3/'},
    &nbsp;&nbsp; 'uri': 'https://api.nephoscale.com/datacenter/zone/87729/',
    &nbsp;&nbsp; 'name': 'RIC-1', 
    &nbsp;&nbsp; 'id': 87729}],
    'response': 200}
    

**Contributing to libcloud**

Having created the compute driver for our cloud provider and having tested it’s functionality, it is time to commit it to libcloud. In this case we need to write tests -ideally for all functionality, add some fixtures and make sure the tests pass. Libcloud contains unit tests for nearly everything and enforces testing for new functionality. Testing on libcloud deserves another post by itself.

Until then,&nbsp;details for contributing can be found can be found [here](https://libcloud.readthedocs.org/en/latest/development.html#contributing "contributing to libcloud").The process involves creating a ticket to libcloud’s[JIRA issue tracker](https://issues.apache.org/jira/browse/LIBCLOUD) and opening a pull request to libcloud’s[github repository](http://github.com/apache/libcloud). The code will be reviewed and most probably comments and suggestions will arise.

So have fun developing your driver!

[**To check this driver in action log in to mist.io**](https://mist.io/ "mist.io homepage")

_Special thanks to [Tomaz](http://www.tomaz.me/ "tomaz blog"), maintainer of liblcoud and it’s bigger laborer of love, for proof reading this post._

