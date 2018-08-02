## Oracle Cloud ##
Oracle cloud UI at [https://console.us-phoenix-1.oraclecloud.com/a/]

### oci command setup ###
The 'oci-cli' tool is a cli for oracle.  Documentation [here](https://docs.cloud.oracle.com/iaas/Content/API/Concepts/cliconcepts.htm)

Here is a simplified install guide.

 * Create a virtual env and install oci-cli

        $ virtuallenv oci
        $ . oci/bin/activate
        $ pip install oci-cli

 * Initial setup.  Set up the 'config' (`~/.oci/config`) and the 'oci-cli-rc' file (`~/.oci/oci_cli_rc`).  

        $ oci setup config

    It will prompt you for a pem file key or generate one for you.  After generating you have to upload it via the web CLI.  

        $ oci oci setup oci-cli-rc

    After the above, you will want to add a `[DEFAULT]` section to the oci-cli-rc file and specify the compartment-id.

    Examples of my files:

        $ cat .oci/config
        [DEFAULT]
        user=ocid1.user.oc1..aaaaaaaaoabcdefghijklmnopqrstuvwxyz0123456789abcdefghijklmno
        fingerprint=43:b6:cd:1e:cf:f5:aa:2b:17:26:d4:18:59:ee:57:ad
        key_file=/home/smoser/data/oracle-cloud/.oci/oci_api_key.pem
        tenancy=ocid1.tenancy.oc1..aaaaaaaaoabcdefghijklmnopqrstuvwxyz0123456789abcdefghijklmno
        region=us-phoenix-1

        $ sed '/^$/q' .oci/oci_cli_rc 
        [DEFAULT]
        # get this from https://console.us-phoenix-1.oraclecloud.com/a/identity/compartments
        compartment-id=ocid1.tenancy.oc1..aaaaaaaaoabcdefghijklmnopqrstuvwxyz0123456789abcdefghijklmno


 * At this point a simple command should work:

        $ oci compute instance list
        ... json stuff here ...

### console output ###
oci cli can get you console output as shown below.
However, you can also get interactive console via ssh

    $ iid=ocid1.instance.oc1.phx.abyhqljt5y6pfuosmdoh3g4n6vx2gk4hs4tnlf6iskbkyk7i72pbug73ncqa;
    $ oci compute instance-console-connection create --instance-id=$iid \
        --ssh-public-key-file=$HOME/.ssh/id_rsa.pub
    {
      "data": {
        "compartment-id": "ocid1.tenancy.oc1..aaaaaaaao7f7cccogqrg5emjxkxmctzbnhl6zdkkx36yq2jgxnm4p5vmysbq",
        "connection-string": "ssh -o ProxyCommand='ssh -W %h:%p -p 443 ocid1.instanceconsoleconnection.oc1.phx.abyhqljtquzofzaacnqrzpghpormjqadae5i6vwluzmxnmkxswwlgwkcrwja@instance-console.us-phoenix-1.oraclecloud.com' ocid1.instance.oc1.phx.abyhqljt5y6pfuosmdoh3g4n6vx2gk4hs4tnlf6iskbkyk7i72pbug73ncqa",
        "defined-tags": {},
        "fingerprint": "SHA256:Y0vcuPDS2HHIIfJ+ZjfO7kGln+ALmAO9hmYJb1B3fqI",
        "freeform-tags": {},
        "id": "ocid1.instanceconsoleconnection.oc1.phx.abyhqljtquzofzaacnqrzpghpormjqadae5i6vwluzmxnmkxswwlgwkcrwja",
        "instance-id": "ocid1.instance.oc1.phx.abyhqljt5y6pfuosmdoh3g4n6vx2gk4hs4tnlf6iskbkyk7i72pbug73ncqa",
        "lifecycle-state": "CREATING",
        "vnc-connection-string": "ssh -o ProxyCommand='ssh -W %h:%p -p 443 ocid1.instanceconsoleconnection.oc1.phx.abyhqljtquzofzaacnqrzpghpormjqadae5i6vwluzmxnmkxswwlgwkcrwja@instance-console.us-phoenix-1.oraclecloud.com' -N -L localhost:5900:ocid1.instance.oc1.phx.abyhqljt5y6pfuosmdoh3g4n6vx2gk4hs4tnlf6iskbkyk7i72pbug73ncqa:5900 ocid1.instance.oc1.phx.abyhqljt5y6pfuosmdoh3g4n6vx2gk4hs4tnlf6iskbkyk7i72pbug73ncqa"
      },
      "etag": "08b6583020bf1421bcb3164cf1ca4690bb7580aa046a034332aa48595dcbfb97"
    }

    $ ssh -o ProxyCommand='ssh -W %h:%p -p 443 ocid1.instanceconsoleconnection.oc1.phx.abyhqljtquzofzaacnqrzpghpormjqadae5i6vwluzmxnmkxswwlgwkcrwja@instance-console.us-phoenix-1.oraclecloud.com' ocid1.instance.oc1.phx.abyhqljt5y6pfuosmdoh3g4n6vx2gk4hs4tnlf6iskbkyk7i72pbug73ncqa

    Ubuntu 16.04.4 LTS fginther-dns-triage ttyS0

    fginther-dns-triage login: 

Then you can delete it using the 'id' from creation.

    $ oci compute instance-console-connection delete  --force --instance-console-connection-id=$ICC_ID


To get a log, like this:

    $ oci compute console-history capture --instance-id=ocid1.instance.o.....ncqa
    {
      "data": {
        "availability-domain": "qIZq:PHX-AD-1",
        "compartment-id": "ocid1.tenancy.oc1..aaaaaaaao7f7cccogqrg5emjxkxmctzbnhl6zdkkx36yq2jgxnm4p5vmysbq",
        "defined-tags": {},
        "display-name": "consolehistory20180802150243",
        "freeform-tags": {},
        "id": "ocid1.consolehistory.oc1.phx.abyhqljtrtuurr3s24e4bddptq7gfptplfhwqkrxaje7tg6lhs5rsgptsaoq",
        "instance-id": "ocid1.instance.oc1.phx.abyhqljt5y6pfuosmdoh3g4n6vx2gk4hs4tnlf6iskbkyk7i72pbug73ncqa",
        "lifecycle-state": "REQUESTED",
        "time-created": "2018-08-02T15:02:43.242000+00:00"
      },
      "etag": "e90f5f494af9a55400f9601a624037ea8004f0662c4fd2e2ccdf81a3978c8fd0"
    }

    # use the 'id' value from above for --instance-console-history-id
    $ oci compute console-history get-content --file=- \
        --length=$((1024*1024)) \
        --instance-console-history-id=ocid1.consolehistory.oc1.p........gptsaoq




