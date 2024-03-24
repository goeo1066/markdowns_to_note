# ArgumentResolver

## Summary & Effect

Map Custom Parameters using HandlerMethodArgumentResolver

## Example

#### SimplePrincipalArgumentResolver.java

```java
...
import org.springframework.web.reactive.result.method.HandlerMethodArgumentResolver;
...

public class SimplePrincipalArgumentResolver implements HandlerMethodArgumentResolver {
    ...
    @Override
    public boolean supportsParameter(MethodParameter parameter) {
        return CustomPrincipal.class.equals(parameter.getParameterType());
    }
    
    @Override
    public Mono<Object> resolveArgument(MethodParameter parameter, BindingContext bindingContext, ServerWebExchange exchange) {
        return ReactiveSecurityContextHolder
                .getContext()
                .map(SecurityContext::getAuthentication)
                .map(Authentication::getPrincipal)
                .switchIfEmpty(Mono.defer(() -> {
                    LOGGER.info("Empty Authentication or Principal");
                    return Mono.empty();
                }));
    }
}
```

#### WebFluxConfig.java

```java
import org.springframework.web.reactive.config.WebFluxConfigurer;

@EnableWebFluxSecurity
@Configuration
public class WebFluxConfig implements WebFluxConfigurer {
    @Override
    public void configureArgumentResolver(ArgumentResolverConfigurer configurer) {
        configurer.addCustomResolver(new SimplePrincipalArgumentResolver());
    }
}
```
