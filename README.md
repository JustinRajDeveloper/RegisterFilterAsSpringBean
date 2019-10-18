# RegisterFilterAsSpringBean

This Doc is use to explain when we want to use Spring beans inside servlet filter. Here we are using DelegatingFilterProxy with Java Configuration.

1. SpringBootServletInitializer - Do below config in ServletInitializer

```
public class RootConfig extends SpringBootServletInitializer {
    @Override
    public void onStartup(ServletContext servletContext) throws ServletException {
       FilterRegistration customFilter =  servletContext.addFilter("customFilter", new DelegatingFilterProxy("customFilter"))
       customFilter.addMappingForUrlPatterns(null, false, "/api/*");
    }
}
```

2. Filter - Create custom filter and add constructor to use spring beans. This constructor should accept all the beans you need to use in Filter

```
public class CustomFilter implements Filter {
    ...
    // No @Autowired needed here
    private CustomObject customObject;
    public CustomFilter(CustomObject customObject) {
        this.customObject = customObject;
    }
}
```

3. Java Configuration - Inject filter Bean

```
public class FilterConfig {
    @Bean
    public CustomFilter customFilter( CustomObject customObject) {
        return new CustomFilter(customObject);
    }
}
```

Hapieee Coding.
