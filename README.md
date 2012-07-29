Registration agent for Difio, preconfigured for OpenShift / JBoss
applications.

It compiles a list of installed Maven dependencies and sends it to http://www.dif.io.


Installing on your OpenShift JBoss application
--------------------------------------------

- Create an account at http://www.dif.io

- Create your JBoss application in OpenShift

        rhc-create-app -a myapp -t jbosseap-6.0

or

        rhc-create-app -a myapp -t jbossas-7


- Download the registration script into your application

        cd myapp/
        wget https://raw.github.com/difio/difio-openshift-java/master/difio-openshift-java -O .openshift/action_hooks/difio-openshift-java
        chmod +x .openshift/action_hooks/difio-openshift-java

- Enable the registration script in `.openshift/action_hooks/post_deploy` and set your userID

        #!/bin/sh
        export DIFIO_USER_ID=YourUserID
        $OPENSHIFT_REPO_DIR/.openshift/action_hooks/difio-openshift-java

- Commit and push your application to OpenShift

        git add . && git commit -m "enable Difio registration" && git push

- If everything goes well you should see something like:

        remote: Running .openshift/action_hooks/post_deploy
        remote: 
        remote: {"message": "Success, registered/updated application 202c430be88c425ea7b3f9e64d2c0483", "exit_code": 0}

- That's it, you can now check your application statistics at http://www.dif.io

Updating the registration agent
-------------------------------

- When a new version of the registration agent script is available simply overwrite your current one

        cd myapp/
        wget https://raw.github.com/difio/difio-openshift-java/master/difio-openshift-java -O .openshift/action_hooks/difio-openshift-java
        chmod +x .openshift/action_hooks/difio-openshift-java
        git add . && git commit -m "updated to latest version of difio-openshift-java" && git push
