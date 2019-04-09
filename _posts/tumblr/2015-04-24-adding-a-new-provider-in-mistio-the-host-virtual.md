---
layout: post
title: 'Adding a new provider in Mist.io: The Host Virtual Case'
date: '2015-04-24T08:41:57+03:00'
tags:
- hostvirtual
- cloud management
- libcloud
tumblr_url: https://blog.mist.io/post/117233356986/adding-a-new-provider-in-mistio-the-host-virtual
---
The Mist.io [open source project](https://github.com/mistio/mist.io), provides a touch-friendly control panel for managing your clouds. Whether you’re using KVM hypervisors, public or private clouds, Docker containers or bare-metal servers, you can manage everything from a single point.

Mist.io supports all popular cloud providers, but there are some that we didn’t get around to adding yet. If your favorite provider is not supported, don’t worry because it’s fairly easy to add a new one. To illustrate this, we’ll walk you through the process of adding a new provider to Mist.io. We’ll use the example of [Host Virtual](https://www.hostvirtual.com), a [highly flexible provider](https://www.hostvirtual.com/why-us/) with datacenters in 20 locations around the world, we’ve recently added support for.

<figure data-orig-width="425" data-orig-height="354" class="tmblr-full"><img src="assets/tumblr-images/tumblr_inline_nnapcxBd8f1rgqrs8_540.png" data-orig-width="425" data-orig-height="354"></figure>

The only prerequisite in order to add a new provider to Mist.io is a libcloud driver for that provider. In case there isn’t one, check this comprehensive [guide](http://blog.mist.io/2013-11-18-how-to-create-a-libcloud-driver-from-scratch) on how to write one. The list of providers supported by libcloud is [really big](https://libcloud.readthedocs.org/en/latest/supported_providers.html) and many are already covered. The list grows fast so before you get your hands dirty send an email to the [mailing list](http://libcloud.readthedocs.org/en/latest/developer_information.html) in case someone is already working on it.

The first step is to fork the mist.io repository on [github](https://github.com/mistio/mist.io). Then install it locally by following the [instructions](https://github.com/mistio/mist.io). Make sure you use the fork’s url where required. After a few minutes Mist.io should be up and running on port 8000.

Now create a new git branch and start hacking:

    $ git branch hostvirtual_provider
    $ git checkout hostvirtual_provider

Time to write some code. Add the new provider to src/mist/io/config.py under the SUPPORTED\_PROVIDERS\_V\_2 section.

The Host Virtual entry looks like this:

    # HostVirtual
    {
        'title': 'HostVirtual',
        'provider' : Provider.HOSTVIRTUAL,
        'regions': []
    }

Please use a descriptive title. The provider attribute, should correspond to the libcloud provider name. You can find a list of all libcloud providers and their names [here](https://github.com/apache/libcloud/blob/trunk/libcloud/compute/types.py). The regions attribute is usually empty and applies only to providers like Amazon which treats regions, eg EC2 Sydney, EC2 Ireland etc, as separate clouds. The attributes ‘title’, ‘provider’ and ‘regions’ are mandatory, but you may add as many additional ones as you need to.

Now edit methods.py and add the provider to the add\_backend\_v\_2 function:

    elif provider == 'hostvirtual':
        backend_id, backend = _add_backend_hostvirtual(title, provider, params)

Define the actual function below:

    def _add_backend_hostvirtual(title, provider, params):
        api_key = params.get('api_key', '')
        if not api_key:
            raise RequiredParameterMissingError('api_key')
        backend = model.Backend()
        backend.title = title
        backend.provider = provider
        backend.apikey = api_key
        backend.apisecret = api_key
        backend.enabled = True
        backend_id = backend.get_id()
        return backend_id, backend

Host Virtual requires only one key for access to its API. Others may require certificates, usernames or other info as you can see on other \_add\_backend functions in the same file. &nbsp;

Now search for the connect\_provider function and pass the API key to the libcloud driver object.

    elif backend.provider in [Provider.LINODE, Provider.HOSTVIRTUAL]:
        conn = driver(backend.apisecret)

Each provider allows you to perform different actions depending on the state of the machine. For example, most cloud providers do not allow starting a terminated machine. Libvirt on the other hand, which is used to manage KVM hypervisors, allows restarting. You can specify the actions that can be performed for each machine state on the get\_machine\_actions function.

In the case of Host Virtual tagging in not supported:

    if conn.type in (Provider.RACKSPACE_FIRST_GEN, Provider.LINODE, Provider.NEPHOSCALE, Provider.SOFTLAYER,Provider.DIGITAL_OCEAN, Provider.DOCKER, Provider.AZURE,Provider.VCLOUD, Provider.INDONESIAN_VCLOUD, Provider.LIBVIRT, Provider.HOSTVIRTUAL):
        can_tag = False

Next step is to write the code for provisioning VMs. This is handled by the create\_machine function. Most providers require name, image, size, location and finally a public SSH key. The latter is used in order to be able to connect to your machine once it is up and running. If you are unsure on what your provider requires, you can check out the corresponding [libcloud driver](https://github.com/apache/libcloud/tree/trunk/libcloud/compute/drivers) and see if the create\_node function takes any extra arguments.

In some cases the provisioning process is complicated, but fortunately Host Virtual’s API is straightforward so you just need to add this:

    elif conn.type == Provider.HOSTVIRTUAL:
        node = _create_machine_hostvirtual(conn, public_key, machine_name, image, size, location)

and the respective creation function:

    def _create_machine_hostvirtual(conn, public_key, machine_name, image, size, location):
        """Create a machine in HostVirtual.
        There is no checking done here, all parameters are expected to be
        sanitized by create_machine.
        """
        key = public_key.replace('\n', '')
        auth = NodeAuthSSHKey(pubkey=key)
        try:
            node = conn.create_node(
            name=machine_name,
            image=image,
            size=size,
            auth=auth,
            location=location
            )
        except Exception as e:
            raise MachineCreationError("HostVirtual, got exception %s" % e, e)
        return node

The final step is to create the relevant “Add Cloud“ form in Mist.io’s UI. Open [src/mist/io/static/js/app.js](https://github.com/mistio/mist.io/blob/master/src/mist/io/static/js/app.js) and find the section starting with “var PROVIDER\_MAP”. For Host Virtual the entry is rather simple, with only a title and and API key field:

    hostvirtual: [
        {
            name: 'title',
            type: 'text',
            defaultValue: 'HostVirtual',
        },
        {
            name: 'api_key',
            type: 'password',
        },
    ],

For the final touches add the provider’s logo. You’ll need a 48x48 png file which you should put in the src/mist//io/static/images/sprite-images directory.

<figure class="tmblr-full" data-orig-height="385" data-orig-width="325"><img src="assets/tumblr-images/tumblr_inline_nnapm9oTTi1rgqrs8_540.png" data-orig-height="385" data-orig-width="325"></figure>

That’s it!

Once you test and make sure everything is working fine, commit the code and push everything to github

    $ git add src/mist//io/static/images/sprite-images/*
    $ git commit -a -m ‘Added hostvirtual provider’
    $ git push -u origin hostvirtual_provider

Open a pull request from mist.io’s staging branch to your fork and branch. We’ll review the code and contact you if we have any questions or suggestions. If everything checks out, we’ll add it to the Mist.io staging branch, test it a bit more and push it to master.

**Are you using Host Virtual? Mist.io can help you manage and monitor your virtual machines from anywhere. [Try it for free](https://mist.io).**

