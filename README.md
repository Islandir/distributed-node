Heading
=======
 
distributed-node
================

h1 | h2
----|---


:smile:

:pizza:

# JSON Configuration Maven Plugin

Generate or edit a JSON configuration object.

## Usage

```
<build>
    <plugins>
        <plugin>
            <artifactId>oracle.otools</artifactId>
            <groupId>json-configuration-plugin</groupId>
            <version>1.0-SNAPSHOT</version>
            <executions>
                <execution>
                    . . .
                </execution>
            </execution>
        </plugin>
    </plugins>
</build>
```

## Goals

### clean 

Delete the JSON file, usually bound to the clean phase in maven.

```
  <execution>
    <phase>clean</phase>
    <goals>
      <goal>clean</goal>
    </goals>
    <configuration>
      <jsonFile>src/js/sample.json</jsonFile>
    </configuration>
  </execution>
```


### init 

Sets the contents of JSON file to an empty object

Add this as the first execution in the build - typically bound to 
generate-sources if the merged file is built from scratch every time.

```
  <execution>
    <phase>generate-sources</phase>
    <goals>
      <goal>init</goal>
    </goals>
    <configuration>
      <jsonFile>src/js/sample.json</jsonFile>
    </configuration>
  </execution>
```

### add, update, remove

Adds (or overwrites), updates or removes a key and value from the JSON object.
Update will fail if key does not already exist.
```
  <execution>
    <phase>generate-sources</phase>
    <goals>
      <goal>add</goal>
    </goals>
    <configuration>
      <jsonFile>src/js/sample.json</jsonFile>
      <key>Key1</key>
      <value>Value1</value>
    </configuration>
  </execution>
  <execution>
    <phase>generate-sources</phase>
    <goals>
      <goal>add</goal>
    </goals>
    <configuration>
      <jsonFile>src/js/sample.json</jsonFile>
      <key>Key2.Key3.Key4</key>
      <value>Value4</value>
    </configuration>
  </execution>
```
generates the json
```
{
  "Key1": "Value1"
  "Key2" : { 
              "Key3" : {
                          "Key4" : "Value4"
                       }
           }
}
```
notice the use of dot notation when specifying nested objects.

To add array elements, use the _add_ goal and prefix value with plus (+)
To remove array elements, use the _remove_ goal and prefix value with minus (-)

```
  <execution>
    <phase>generate-sources</phase>
    <goals>
      <goal>add</goal>
    </goals>
    <configuration>
      <jsonFile>src/js/sample.json</jsonFile>
      <key>foo</key>
      <value>+elem1</value>
    </configuration>
  </execution>

```

generates 

```
{ 
  "foo" : [ "elem1"]
}
```

### merge, unmerge

Merge contents from one JSON file _into_ another. Unmerge to reverse it.

```
  <execution>
    <phase>generate-sources</phase>
    <goals>
      <goal>merge</goal>
    </goals>
    <configuration>
      <toJsonFile>src/js/combined.json</jsonFile>
      <fromJsonFile>src/js/modules/module1.json</fromJsonFile>
    </configuration>
  </execution>
  <execution>
    <phase>generate-sources</phase>
    <goals>
      <goal>merge</goal>
    </goals>
    <configuration>
      <toJsonFile>src/js/combined.json</jsonFile>
      <fromJsonFile>src/js/modules/module2.json</fromJsonFile>
    </configuration>
  </execution>

```
