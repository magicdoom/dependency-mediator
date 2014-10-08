# Dependency mediator project

#### Unlike karaf and other lightness OSGI solutions,dependency mediator focus on shooting the conflict or incompatible problems before the runtime(try to remedy some unexpected exceptions or errors),also it can help you find the compatible problem between any two java component.
#### Nowadays,I have provided one maven plugin.but in my opinion,integration with the maven standard enforcer plugin may be a better choice, i would try in the near future. 

## Available version
0.1.0 snapshot release on 2012.9.12   
1.0.0 snapshot release on 2014.9.19   

## Features
#### 1.Compatible with maven 3.x.x plugin programming model
#### 2.Compatible with JDK 6+
#### 3.Support directory scan,including classpath
#### 4.Support component scan,including jar,war,ear,sar and so on
#### 5.Support duplicate classes scan,duplicate means the same fully-qualified class name, but not the same digest or incompatible class(details see [jls](http://docs.oracle.com/javase/specs/jls/se7/html/jls-13.html) and [class compatibility](http://www.oracle.com/technetwork/java/javase/compatibility-137541.html))

## How to Use

### Maven plugin(Compatible with maven 3.x.x)
	<plugin>
		<groupId>org.apache.component</groupId>
		<artifactId>dependency-mediator-maven-plugin</artifactId>
		<version>1.0.0-SNAPSHOT</version>
	</plugin>

you can also add plugin's groupId to the list of groupIds searched by default. To do this, you need to add the following to your ${user.home}/.m2/settings.xml file:

    <pluginGroups>
       <pluginGroup>org.apache.component</pluginGroup>
    </pluginGroups>

finally,you can run the mojo with ***mvn mediator:check***


### Standalone 
After import the following jar

    <dependency>
       <groupId>org.apache.component</groupId>
	   <artifactId>dependency-mediator-core</artifactId>
	   <version>1.0.0-SNAPSHOT</version>
	</dependency>
	
You can invoke the command ***mvn exec:java -Dexec.args="scanWhere -DscanClasspath"*** in maven project or invoke class DependencyMediator
## Usecase
Output may be like this if you use standalone mode:
   
    Output component reactor info......
    Duplicated component  [com.alibaba.rocketmq.storm.MessageConsumer] was founded in the  path : 
 	    /home/von/workspace/rocketmq-storm/dd/rocketmq-storm-1.0.0-SNAPSHOT-11/com/alibaba/rocketmq/storm/MessageConsumer.class
 	    /home/von/workspace/rocketmq-storm/dd/rocketmq-storm-1.0.0-SNAPSHOT/com/alibaba/rocketmq/storm/MessageConsumer.class
 	    
 	    
But if you using maven plugin,ouput may be like this:

    [WARNING] Founded conflict dependency components:org.apache.commons:commons-lang3:jar
     Resolved version is org.apache.commons:commons-lang3:jar:3.1:compile
     But found conflict artifact org.apache.commons:commons-lang3:3.3.2
    [WARNING] Founded conflict dependency components:org.apache.thrift:libthrift:jar
     Resolved version is org.apache.thrift:libthrift:jar:0.8.0:compile
     But found conflict artifact org.apache.thrift:libthrift:0.9.1


## Background 

As we know,when we are building a web project,we often use maven dependency plugin(if maven project) to solve jar conflict,such as:mvn dependency:tree -Dverbose -Dincludes=commons-collections,
but if we build our project to war package according with Java EE specification.we always have nothing to do but with the naked eye to lookup some underlying conflict jar packages.of course,which 
depend on Java EE container classloader's class loading mechanism.

Now,dependency mediator can help you to solve this problem,if you have better idea or improving suggestion,please contact [me](fengjia10@gmail.com)