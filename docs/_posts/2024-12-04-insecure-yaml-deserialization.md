---
title: "Risks With Insecure YAML Serialization In Python"
date: 2024-12-04
categories: 
    - Security
    - Python
    - Yaml
tags: 
    - python
    - yaml
    - security
    - serialization
    - deserialization
description: "Learn about the security risks associated with YAML serialization in Python and how to protect your applications from potential exploits."
published: true
---

YAML is a Python favorite for object serialization, especially around application configuration. So, it is good to be aware that you can expose yourself to security risks when using YAML and know how to avoid those pitfalls.

## Why Could Using YAML In Python Cause a Security Risk?

The first thing to understand is that these risks are not inherent to our choice of Python, or even YAML. The risk comes from exploits of insecure deserialization. These exploits are pervasive across serialization formats, programming languages and serialization libraries. For a noteworthy example, a Java/XML variant of an insecure deserialization vulnerability was exploited for the 2017 Equifax data breach.

Deserialization can be a weak spot because to fully restore objects from their encoded state, deserialization libraries need to run the initializers those objects provide. Executing those initializers leaves many avenues available for an attacker to execute harmful code or take varying degrees of control of a target application's server.

## How Can Your Code Be Attacked?

Attacks using insecure deserialization exploits range from tampering with attribute values, modifying application behavior, all the way to arbitrary code execution and compromising an entire server. Here are two factors leading to vulnerability:

1. **Insecure Source Data**: The application code in some way accepts serialized data that contains an exploit from an insecure source, such as inter-process communication or persistent storage. It may be a source the user mistakenly trusts. This is the entry point.

2. **Code Execution**: The library responsible for deserializing the object allows the object itself to execute code – an exploit approach sometimes known as a "hook method." The hook methods are often invoked when deserialization reconstitutes user-defined objects into memory. When this happens, the attacks fully execute before deserialization finishes – they don't even require successful deserialization.

For less intrusive attacks, the attacker would only need the first of these factors to hold. For example, intercepting and modifying the state of an application object retrieved from a cache or a session cookie could allow an attacker to change a user role to grant unauthorized permissions.

When insecure data and code execution risks exist together, they enable more sophisticated and nefarious attacks. They can have more powerful impacts such as performing arbitrary code execution or even allowing attackers to execute commands on the application server with administrative privileges.

Gadget chains are one example of exploits that take advantage of deserialization-triggered code execution, usually through a hook method. Attackers construct a gadget chain by creating a serialized payload composed of a series of objects linked by method invocations, going from a starting "kick-off" gadget invoked from a hook method, through a chain of objects to a more powerful "sink" gadget that has the privileges to execute a system call or other dangerous command.

All objects used in the gadget chain must originate in modules loaded and available to the target application at runtime. In Python this would be any classes imported through the import graph of the target application's `__main__` module or available through the Python standard library. While it helps to have access to the source code of the target application to learn what classes are available for a gadget chain exploit, it is not necessary. A patient attacker can make assumptions and send some probing payloads just to ensure all parts of the chain will work.

## A Concrete Example with Python and PyYAML

Here is a simple test you can try leveraging a deserialization exploit drawn from the guide "YAML Deserialization Attack in Python" by Manmeet Singh and Ashish Kukreti. Newer versions of the PyYAML package have patched this attack for their default deserialization method, but it is still possible to fall victim when using a different 'Loader' class, as below:

Create a serialized object wrapping the malicious behavior:

```python
import yaml
import subprocess

class Payload():
    def __reduce__(self):
        return (subprocess.Popen, ("ls",))

deserialized_data = yaml.dump(Payload())  # serializing data
print(deserialized_data)
```

Running this script in your shell yields the serialized YAML object:

```yaml
!!python/object/apply:subprocess.Popen
- ls
```

Building on our example, now let's pretend we are in a running application. The below script executes a call to load this data and unknowingly executes the attack. We directly pass the byte string in this case, but in an actual attack the application would take this in through something like data transfer or file I/O:

```python
from yaml import *

data = b"""!!python/object/apply:subprocess.Popen
- ls"""
deserialized_data = load(data, Loader=Loader)  # deserializing data
print(deserialized_data)
```

Running this program will cause an attempt to deserialize the YAML and in the process it will list the contents of the script's directory. I have tested successfully with PyYAML version 6.0, the latest version of PyYAML as of this writing. Here is a mocked up example of what you might see in your console as output:

```
<Popen: returncode: None args: 'ls'>
application_src_folder application_resource_folder config_file.yml
data                   requirements.txt            setup.py
```

Of note here: in this example, in the call to `load()`, we set the `Loader` keyword argument to `Loader` in the call to load. For earlier versions of PyYAML, you would not be required to pass a Loader class and if you omitted it a default loader was used. The 'Loader' we use here is the default loader from PyYAML in versions less than 5.1, which is unsafe and allows trivial arbitrary code execution exploits like the one above.

How does PyYAML protect against this now? The default loader from PyYAML 5.1 up to 6.0 is called `FullLoader`. In this example, FullLoader would block this attack because it attempts to import and use the subprocess module which was never imported in our example application script. Despite this check and other safeguards, FullLoader still uses the full functionality of the YAML spec and therefore can still fall prey to more sophisticated exploits such as gadget chains in commonly used modules. The recommended default loader for PyYAML is SafeLoader, which we will discuss in the next section on staying safe.

## What Measures Can You Take To Stay Safe?

Here are two main ways to stay safe from insecure deserialization:

1. Avoid deserializing objects from sources you cannot verify. Whether it is a message sent from a procedure call or byte stream read in from a file, work to build in some layer of trust so you can be sure the payload is not malicious.

2. Restrict your application's deserialization capability.

This involves thinking about your specific use case and what you really need. For example, you may not be concerned about insecure YAML deserialization if you are using it to read objects from a file you create and deploy alongside your code to a very secure environment. In that case, if someone compromises your security enough to be able to intercept and modify the file contents, they also have easier ways to wreak havoc on your system. Just be mindful that having a part of your application that allows arbitrary code execution plants a vulnerability. Potential attackers may have unlimited retries, and only have to be successful once.

Additionally, PyYAML provides a `safe_load` method that uses their 'SafeLoader' class, which is the recommended method from the version 6.0 docs. Loading in this manner only uses the standard YAML tags and refuses to construct arbitrary Python objects. With SafeLoader you can only read data objects composed of Python primitives and basic data structures, which is all that is needed in most cases. If you really need state and behavior, then PyYAML also provides a path for you to derive from the YAMLObject class and build safe user-defined types that can be recognized by SafeLoader.

Thanks for reading and stay safe!