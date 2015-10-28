# maven-transitive-dependencies
Playing with maven transitive dependency resolution

Consider the following 3 maven modules: `app`, `module1`, `module2`

* `module1` declares a `recommended` dependency to log4j version 1.2.12
* `module2` declares a `fixed` dependency to log4j version 1.2.12 using the square brackets around the version of `log4j`

```
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>[1.2.12]</version>
        </dependency>
```

We want to see which version of the `log4j` transitive dependency is pulled in 
when declaring `module1` or `module2` as a dependency for the `app` module

### Declaring `module1` as a dependency for the `app` module

the `log4j` 1.2.12 is pulled in by default, but can be overriden
if we specify a newer version of `log4j`. `The order does not matter`, maven is choosing the smallest distance 
from the root of the tree

```
[INFO] org.example:app:war:1.0-SNAPSHOT
[INFO] +- log4j:log4j:jar:1.2.17:compile
[INFO] \- org.example:module1:jar:1.0-SNAPSHOT:compile
```
```
[INFO] org.example:app:war:1.0-SNAPSHOT
[INFO] +- org.example:module1:jar:1.0-SNAPSHOT:compile
[INFO] \- log4j:log4j:jar:1.2.17:compile
```

### Declaring `module2` as a dependency for the `app` module

the `log4j` 1.2.12 is pulled in, and it `cannot be overriden` even if you declare the `log4j` dependency in the `app` module's `pom.xml`
