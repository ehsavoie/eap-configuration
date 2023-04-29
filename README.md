# eap-configuration

Using GIT remote repo to Manage JBoss EAP configuration data (Based on https://access.redhat.com/documentation/en-us/red_hat_jboss_enterprise_application_platform/7.4/html/configuration_guide/jboss_eap_management#using_git_to_manage_configuration_data)

1. Create git repo
2. Create Git token (https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token)
3. In local machine, create `github-wildfly-config.xml`:
~~~
<?xml version="1.0" encoding="UTF-8"?>
<configuration>
  <authentication-client xmlns="urn:elytron:client:1.2">
    <authentication-rules>
      <rule use-configuration="test-login">
      </rule>
    </authentication-rules>
    <authentication-configurations>
      <configuration name="test-login">
        <sasl-mechanism-selector selector="BASIC" />
        <set-user-name name="admin" />
        <credentials>
          <clear-password password="PUT_HERE_YOUR_GENERATED_TOKEN" />
        </credentials>
        <set-mechanism-realm name="testRealm" />
      </configuration>
    </authentication-configurations>
  </authentication-client>
</configuration>
~~~
4. Go to JBOSS_HOME/standalone and push the `configuration` dir to the repo (this steps is only to push Configuration chages):
~~~
 cd JBOSS_HOME/standalone
 git init
 git add configuration 
 git commit -am "EAP configuration test"
 git branch -M main
 git remote add origin https://github.com/alexbarbosa1989/eap-configuration.git
 git push -u origin main
~~~
5. Go to JBOSS_EAP home and start the JBoss instance with the Git repo files
~~~
cd JBOSS_HOME
./bin/standalone.sh --git-repo=https://github.com/alexbarbosa1989/eap-configuration.git --git-branch=main --git-auth=file:///opt/gitEAPtest/github-wildfly-config.xml --server-config=standalone-full.xml
~~~


