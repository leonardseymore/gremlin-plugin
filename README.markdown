This is a [Neo4j Server](http://neo4j.org/download) plugin, providing [Gremlin](http://gremlin.tinkerpop.com) backend scripting to the [Neo4j Server](http://neo4j.org). To deploy, please do the following

For usage, see the [current documentation](http://neo4j-contrib.github.io/gremlin-plugin/).

Building from source and deploying into Neo4j Server
-----------------------------------------------------

    mvn clean package
    unzip target/neo4j-gremlin-plugin-2.0-SNAPSHOT-server-plugin.zip -d $NEO4J_HOME/plugins/gremlin-plugin
    cd $NEO4J_HOME
    bin/neo4j restart

Maven setup
-----------

In your `pom.xml`, add    

    <repositories>
        <repository>
            <id>neo4j-contrib-snapshots</id>
            <url>https://github.com/neo4j-contrib/m2/raw/master/snapshots/url>
            <snapshots>
                <enabled>true</enabled>
            </snapshots>
        </repository>
    </repositories>

and

    <dependency>
         <groupId>org.neo4j.server.plugin</groupId>
         <artifactId>neo4j-gremlin-plugin</artifactId>
         <version>2.0-SNAPSHOT</version>
         <type>test-jar</type>
         <scope>test</scope>
    </dependency>


    
Eclipse setup
-------------

* Install m2eclipse maven support for Eclipse from the update site [http://download.eclipse.org/technology/m2e/releases](http://download.eclipse.org/technology/m2e/releases)
* clone this repo
* do Eclipse->Import...->Maven->Existing_projects_into_workspace and point out your cloned code directory
* wait until the process is finished and you should have a compiling setup in Eclipse.
  
    
access the plugin

    curl localhost:7474/db/data/
    {
    "relationship_index" : "http://localhost:7474/db/data/index/relationship",
    "node" : "http://localhost:7474/db/data/node",
    "relationship_types" : "http://localhost:7474/db/data/relationship/types",
    "extensions_info" : "http://localhost:7474/db/data/ext",
    "node_index" : "http://localhost:7474/db/data/index/node",
    "reference_node" : "http://localhost:7474/db/data/node/0",
    "extensions" : {
      "GremlinPlugin" : {
        "execute_script" : "http://localhost:7474/db/data/ext/GremlinPlugin/graphdb/execute_script"
      }
    }


submit (HTTP POST) a Gremlin script `i=g.V(2);i.outE.inV` returning a list of nodes, URL encoded:

    curl -d "script=i+%3D+g.v%282%29%3Bi.outE.inV" http://localhost:7474/db/data/ext/GremlinPlugin/graphdb/execute_script
    [ {
      "outgoing_relationships" : "http://localhost:7474/db/data/node/0/relationships/out",
      "data" : {
      },
      "traverse" : "http://localhost:7474/db/data/node/0/traverse/{returnType}",
      "all_typed_relationships" : "http://localhost:7474/db/data/node/0/relationships/all/{-list|&|types}",
      "property" : "http://localhost:7474/db/data/node/0/properties/{key}",
      "self" : "http://localhost:7474/db/data/node/0",
      "properties" : "http://localhost:7474/db/data/node/0/properties",
      "outgoing_typed_relationships" : "http://localhost:7474/db/data/node/0/relationships/out/{-list|&|types}",
      "incoming_relationships" : "http://localhost:7474/db/data/node/0/relationships/in",
      "extensions" : {
      },
      "create_relationship" : "http://localhost:7474/db/data/node/0/relationships",
      "all_relationships" : "http://localhost:7474/db/data/node/0/relationships/all",
      "incoming_typed_relationships" : "http://localhost:7474/db/data/node/0/relationships/in/{-list|&|types}"
    } ]