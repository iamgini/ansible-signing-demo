
Expectation: working without signing

```
 $  ansible-galaxy collection install iamgini.signing_demo -c
Starting galaxy collection install process
Process install dependency map
Starting collection install process
Downloading https://automationhub22-1.lab.local/api/galaxy/v3/plugin/ansible/content/published/collections/artifacts/iamgini-signing_demo-1.0.1.tar.gz to /home/gmadappa/.ansible/tmp/ansible-local-15238mmzoqtxu/tmpbyvyg324/iamgini-signing_demo-1.0.1-hprh_gqe
Installing 'iamgini.signing_demo:1.0.1' to '/home/gmadappa/.ansible/collections/ansible_collections/iamgini/signing_demo'
iamgini.signing_demo:1.0.1 was installed successfully
```


Issue:

```
$  ansible-galaxy collection install iamgini.signing_demo --force -c
Starting galaxy collection install process
Process install dependency map
Starting collection install process
ERROR! Unexpected Exception, this is probably a bug: The is no known source for iamgini.signing_demo:1.0.2
to see the full traceback, use -vvv
```