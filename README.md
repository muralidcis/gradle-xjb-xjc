# gradle-xjb-xjc
Gradle with xjb and xjc examples

gradle-jaxb-plugin
==================


There are only two tasks.

* `xjc`
	- runs `xjc` ant task on each `.xsd` in the dependency tree.
	- needs to be run **manually**.
* `xsd-dependency-tree`
	- Builds a dependency tree from all `.xsd` files configured to be
      parsed.
	- Finds each unique namespace and groups files containing that
      namespace
	- Analyzes xsd dependencies and places them in the correct place
      in the dependency tree so that the namespaces can be parsed in
      order using their generated episode files to bind, allowing
      other projects to use the episode files generated from the
      namespace in the tree.
	  - This keeps all namespaces decoupled and prevents a big
        episode blob containing everything that was parsed.

`xjc` depends on `xsd-dependency-tree` so you don't need to run the
tree task at all.

Plugin Conventions
==================

There are two conventions that can be overridden and one is nested in
the other.

The `jaxb` convention defines the conventions for the whole plugin,
and the `xjc` convention defines the conventions for the `xjc` ant
task.

You can change these defaults with a closure in your build
script.

```groovy
    jaxb {
	  ...
      xjc {
	    ...
	  }
    }
```

## JAXB Plugin Convention ##

There are 4 overrideable defaults for this JAXB Plugin.
These defaults are changed via the `jaxb` closure.

* `xsdDir`
  * **ALWAYS** relative to `project.rootDir`
	* Defined **by each** project to tell the plugin where to find the
      `.xsd` files to parse
* `episodesDir`
  * **ALWAYS** relative to `project.rootDir`
	* i.e. _"episodes"_, _"schema/episodes"_, _"xsd/episodes"_,
      _"XMLSchema/episodes"_
	* **All** generated episode files go directly under here, no subfolders.
* `bindingsDir`
  * **ALWAYS** relative to `project.rootDir`
	* i.e. "bindings", "schema/bindings", "xsd/bindings",
      "XMLSchema/bindings"
    * User defined binding files to pass in to the `xjc` task
	* **All** files are directly under this folder, _no subfolders_.
* `bindings`
  * customization files to bind with
  * file name List of strings found in `bindingsDir`
  * if there are bindings present, xjc will be called once with the
    glob pattern `**/*.xsd` to snag everything under `xsdDir`.  Fixes
    #27.

## XJC Convention ##

These defaults are changed via the nested `xjc` closure.
Several sensible defaults are defined to be passed into the
`wsimport` task:

| parameter				 | Description									    | default		  | type	  |
| :---					 | :---:										    | :---:			  | ---:	  |
|`destinationDir` _(R)_	 | generated code will be written to this directory | `src/main/java` | `String`  |
|`extension` _(O)_		 | Run XJC compiler in extension mode			    | `true`		  | `boolean` |
|`header` _(O)_			 | generates a header in each generated file	    | `true`		  | `boolean` |
|`producesDir` _(O)(NI)_ | aids with XJC up-to-date check				    | `src/main/java` | `String`  |
|`generatePackage` _(O)_ | specify a package to generate to				    | **none**		  | `String`  |
|`args` _(O)_ | List of strings for extra arguments to pass that aren't listed | **none** | `List<String>` |
|`removeOldOutput` _(O)_ | Only used with nested `<produces>` elements, when _'yes'_ all files are deleted before XJC is run | _'yes'_ | `String` |
|`taskClassname` _(O)_ | Enables a custom task classname if using something other than jaxb | `com.sun.tools.xjc.XJCTask` | `String` |

* (O) - optional argument
* (R) - required argument
* (NI) - not implemented / not working

For more in depth description please see
https://jaxb.java.net/2.2.7/docs/ch04.html#tools-xjc-ant-task -- or
substitute the version you are using.

### destinationDir ###

`destinationDir` is relative to `project.projectDir`.  It is
defaulted to `src/main/java`, but can be set to anywhere in
`project.projectDir`.

### producesDir ###

`producesDir` is not currently used in the plugin.  But it was meant
to be in there so that if no xsd changes have happened, then no
code generation would take place.  *Hasn't worked yet.*

### taskClassname ###

`taskClassname` is the class to be used to run the xjc task.  Useful if
JAXB2 is desired to be used.

### Extra Arguments ###
`args` passes arbitrary arguments to the `xjc` ant task. This is useful
when activating JAXB2 plugins.

# Examples #

## Default Example using JAXB ##

If the default conventions aren't changed, the only thing to configure
_(per project)_ is the `xsdDir`, and jaxb dependencies as described above.

```groovy
jaxb {
  xsdDir = "schema/folder1"
}
```

## Default Example using JAXB2 ##

Customized to use the same `xjc` task that
[xjc-mojo](http://mojo.codehaus.org/jaxb2-maven-plugin/xjc-mojo.html)
uses.

```groovy
dependencies {
  jaxb "org.jvnet.jaxb2_commons:jaxb2-basics-ant:0.6.5"
  jaxb "org.jvnet.jaxb2_commons:jaxb2-basics:0.6.4"
  jaxb "org.jvnet.jaxb2_commons:jaxb2-basics-annotate:0.6.4"
}

jaxb {
  xsdDir = "some/folder"
  xjc {
     taskClassname      = "org.jvnet.jaxb2_commons.xjc.XJC2Task"
	 generatePackage    = "com.company.example"
     args               = ["-Xinheritance", "-Xannotate"]
  }
}
```

