# Description

[Thruk](https://www.thruk.org/) is a widely used tool which serves the purpose
of aggregating the information generated by several
[Nagios](https://www.nagios.org/) servers into a single application.

It does this by interfacing with another thruk server running alongside each
nagios server. Each of these remote thruks are called "peers" in thruk's
configuration. A peer is just the URL where the remote thruk lives and a couple
of other pieces of info.

This charm, if deployed with the `meyer91/thruk` image, will create a pod
running said image and automatically start up thruk. The config property
`peers` shall be used to configure all of this thruk's peers.

# Usage

*NOTE:* The set of commands showed below is just indicative, as at the time of
writing there is no nagios charm implemented for the Kubernetes platform yet.

```
    juju deploy nagios --config enable_livestatus=true
    cat >config.yaml <<EOF
    peers:
        peer1":
            nagios_context: peer-1
            url: http://peer1.com
            thruk_key: 94dlks
        peer2:
            nagios_context: peer-2
            url: http://peer2.com
            thruk_key: uoo39og
    EOF
    juju deploy thruk-master --config ./config.yaml --resource thruk-image=meyer91/thruk
```

To access the Web UI visit the url:

    http://<thruk-master-ip>/thruk/

Login with user `thrukadmin`. Its password can be retrieved with

```
    juju ssh --container thruk thruk/0 -- sudo cat /var/lib/thruk/thrukadmin.passwd
```

# Developing

Create and activate a virtualenv with the development requirements:

    virtualenv -p python3 venv
    source venv/bin/activate
    pip install -r requirements-dev.txt

# Testing

The Python operator framework includes a very nice harness for testing
operator behaviour without full deployment. Just `run_tests`:

    ./run_tests
