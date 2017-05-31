ParameterDescription
```java
public static void validateValueParameter(Class<? extends IValueValidator> validator,
      String name, Object value) {
    try {
      if (validator != NoValueValidator.class) {
        p("Validating value parameter:" + name + " value:" + value + " validator:" + validator);
      }
      validator.newInstance().validate(name, value);
    } catch (InstantiationException | IllegalAccessException e) {
      throw new ParameterException("Can't instantiate validator:" + e);
    }
  }
```

JCommander
```java
                    // Regular (non-command) parsing
                    //
                    List mp = getMainParameter(arg);
                    String value = a; // If there's a non-quoted version, prefer that one
                    Object convertedValue = value;

                    if (mainParameter.getGenericType() instanceof ParameterizedType) {
                        ParameterizedType p = (ParameterizedType) mainParameter.getGenericType();
                        Type cls = p.getActualTypeArguments()[0];
                        if (cls instanceof Class) {
                            convertedValue = convertValue(mainParameter, (Class) cls, null, value);
                        }
                    }
                    
                    for(final Class<? extends IParameterValidator> validator : mainParameterAnnotation.validateWith() ) {
                        ParameterDescription.validateParameter(mainParameterDescription,
                        	validator,
                            "Default", value);
                    }

                    mainParameterDescription.setAssigned(true);
                    mp.add(convertedValue);
```

JCommander
==========

This is an annotation based parameter parsing framework for Java 8.

Here is a quick example:

```java
public class JCommanderTest {
    @Parameter
    public List<String> parameters = Lists.newArrayList();
 
    @Parameter(names = { "-log", "-verbose" }, description = "Level of verbosity")
    public Integer verbose = 1;
 
    @Parameter(names = "-groups", description = "Comma-separated list of group names to be run")
    public String groups;
 
    @Parameter(names = "-debug", description = "Debug mode")
    public boolean debug = false;

    @DynamicParameter(names = "-D", description = "Dynamic parameters go here")
    public Map<String, String> dynamicParams = new HashMap<String, String>();

}
```

and how you use it:

```java
JCommanderTest jct = new JCommanderTest();
String[] argv = { "-log", "2", "-groups", "unit1,unit2,unit3",
                    "-debug", "-Doption=value", "a", "b", "c" };
new JCommander(jct, argv);

Assert.assertEquals(2, jct.verbose.intValue());
Assert.assertEquals("unit1,unit2,unit3", jct.groups);
Assert.assertEquals(true, jct.debug);
Assert.assertEquals("value", jct.dynamicParams.get("option"));
Assert.assertEquals(Arrays.asList("a", "b", "c"), jct.parameters);
```

The full doc is available at [http://jcommander.org](http://jcommander.org).

## Building JCommander

```
./kobaltw assemble
```

