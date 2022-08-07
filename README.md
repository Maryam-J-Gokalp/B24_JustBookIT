# B24_JustBookIT

Updating .properties file in cucumber Project

so we have move everyhing like url,baseurl,db connection string,valid credentials for each user type to new qa1,qa2,qa3.properties file that we created under resources/Environments folder.

the only info we left in configuration.properties file is 

browser=chrome
environment=qa2

then we have created new Environment class under Utilities package where we have very similar static block with ConfigurationReader. the only difference is we create dynamic path in this new class. it is accepting environment value from our configuration.properties file and generating new path. so based on the environment we have it will load that env file.example

if conf is following;

browser=chrome
environment=qa2

it will create path as following
environment=ConfiguraitonReader.get("enviroment") --> this will get env from configuraiton.properties file 

String path = System.getProperty("user.dir") + "/src/test/resources/Environments/" + environment + ".properties"; --> variable will update and new path will be generated

then we have created constant variables to load our env specific properties files so that we can just call them whenever we need it instead of using getters of properties.

public static final String URL;

URL = properties.getProperty("url");

Environment.URL --> usage 

Scenario: we have set our environment in configuraiton.propeties file, can we also set it with terminal ? 

so make this happen we update one line in Enviroment class that we added;

it was like this before:
//String environment = ConfigurationReader.get("environment");

then switch to following:

String environment = System.getProperty("environment") != null ? environment = System.getProperty("environment") : ConfigurationReader.get("environment");

so the logic is: if there is enviroment passed through command line, we will use that value to assifn enviroment. if not it will get it from ConfigurationReader file just like before.

command for passing through terminal:

mvn test -Denvironment=qa2

if you dont have maven locally use CMD+enter or CTRL+ enter to run with terminal.

This is useful, when we have jenkins job ready, and we only want to switch environment.
