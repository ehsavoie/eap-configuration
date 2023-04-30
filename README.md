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
4. Pull/Push JBoss EAP Standalone `configuration` files:

4.a. Initial commit: This step is made just once (already done in current repo):
~~~
 cd $JBOSS_HOME/standalone
 git init
 git add configuration 
 git commit -am "EAP configuration test"
 git branch -M main
 git remote add origin https://github.com/alexbarbosa1989/eap-configuration.git
 git push -u origin main
~~~

4.b. Once the repo has the `configuration` directory, You just need to clone it into $JBOSS_HOME/standalone dir:
~~~
 cd $JBOSS_HOME/standalone
 git clone https://github.com/alexbarbosa1989/eap-configuration.git
~~~

5. Go to JBOSS_EAP home and start the JBoss instance with the Git repo files
~~~
cd JBOSS_HOME
./bin/standalone.sh --git-repo=https://github.com/alexbarbosa1989/eap-configuration.git --git-branch=main --git-auth=file:///opt/gitEAPtest/github-wildfly-config.xml --server-config=standalone-full.xml
~~~



