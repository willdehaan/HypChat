=======
HypChat
=======
A Python package for HipChat's `v2 JSON REST API`_. It's based on v2's navigability and self-declaration.

.. _v2 JSON REST API: https://www.hipchat.com/docs/apiv2

Concepts
========

There are two basic types in HypChat: ``Linker`` and ``JsonObject``. They are not meant to be instantiated directly but instead created as references from other objects.

Linker
------
A simple callable that represents an unfollowed reference.

``l.url``
	The URL this object points to

``l()``
	Calling a ``Linker`` will perform the request and return a ``JsonObject``

JsonObject
----------
A subclass of ``dict``, contains additional functionality for links and actions.

Links
~~~~~
As part of the v2 API, all objects have a ``links`` property with references to other objects. This is used to create ``Linker`` objects as attributes.

For example, all objects have a link called ``self``. This may be referenced as:
::

	obj.self

And the request performed by calling it:
::

	obj.self()

If `Title Expansion`_ is desired, just past a list of things to be expanded as the ``expand`` keyword argument.

.. _Title Expansion: https://www.hipchat.com/docs/apiv2/expansion

Other Actions
~~~~~~~~~~~~~

Many of the v2 types define additional types, eg Rooms have methods for messaging, setting the topic, getting the history, and inviting users to the room. These are implemented as methods of subclasses. The complete listing is in the `Type List`_.

Usage
=====

First, create a HypChat object with the token

::

	hc = HypChat("mytoken")

There are several root links:

::

	rooms = hc.rooms()
	users = hc.users()
	emots = hc.emoticons()
	caps = hc.capabilities()

In addition, the HypChat object has methods for creating objects and directly referencing the basic types.

Navigation
----------
Any time an object is referenced in a value (eg ``room['owner']``), a stub of that object is created, and the full object may be found with ``.self()``. Stubs contain the ID of the object, the name (if applicable), and any links that object has—including ``self``.

Collections—such as ``rooms``, ``users``, and ``emots`` above—all have an ``'items'`` key containing their list of things. In addition, the ``.contents()`` method will generate all of the items, handling pagination. As usual, 

Type List
=========
TODO