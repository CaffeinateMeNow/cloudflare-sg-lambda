cloudflare-sg-lambda
====================

This repo documents an AWS Lambda used to maintain a security group
of IP addresses for restricting inbound traffic to Cloudflare only.

As of early 2021, the use of the Python 2.7 runtime has been
deprecated, and so, consequently, is the use of `from
botocore.vendored import requests`. As a result, instead of installing
the bare `lambda_function.py`, we have to install a zipped package,
produced like this:

    pip install --target ./package -r requirements.txt
    cd package/
    zip -r ../deployment-package-`git rev-parse --short HEAD`.zip .
    cd ..
    zip -g deployment-package-`git rev-parse --short HEAD`.zip lambda_function.py

Now you can upload `deployment-package-80b467a.zip` (or whatever it
is), set the environment variables `PORTS_LIST` and
`SECURITY_GROUP_ID`, and set up an EventBridge or other trigger.

To pin requirements, after, for instance, upgrading requests, run

    pip freeze --path package/ > requirements.txt

Source(s) of truth
------------------

This code uses https://api.cloudflare.com/client/v4/ips; as of April
13, 2021, this URL has not caught up with
https://www.cloudflare.com/ips/, which contains a change announced on
April 8, to be made on May 7. 

A variant
---------

This repo contains [a variant](lambda_function_urllib.py) that uses
`urllib` instead of `requests`, and so does not need to be packaged in
the same way.
