---
layout: page
title: "API"
---
{% include api.html %}


# General Concepts

## Thing


## Person



## Property


## PropertyType


# Sign up

We now want to register an account for a cloud service. We will use the prototype
from TU Delft IDE's Faculty of the Data-Centric Design Hub. Go to <a href="https://dwd.tudelft.nl/manager" target="_blank">https://dwd.tudelft.nl/manager</a>

![Flowchart Push Button](/docs/assets/res/dcdhub.png)

Sign up as a group with an email address, a name and a password.

![Flowchart Push Button](/docs/assets/res/signup.png)

The sign up process creates an account, then the standard OAuth2 process starts
with a consent: you need to let the manager access your Things, so that it can
help you manage them. To do so click "Allow access".

![Flowchart Push Button](/docs/assets/res/consent.png)

Once the consent succeeded, you can click on 'My Things' and create a first one.
For example with the name 'My wheelchair', type 'Wheelchair', and a
description 'An Internet-connected wheelchair'.

The process may take a few seconds, as the hub generates an access token for your Thing.

![Flowchart Push Button](/docs/assets/res/create_thing.png)

**COPY AND SAVE THIS TOKEN** in a text file, it will be shown only once and enables
your wheelchair to communicate with the hub. You can also save the thing id, but
you can always go back to the manager to retrieve this id.

Back to Atom and your project, let's create a first Python example.