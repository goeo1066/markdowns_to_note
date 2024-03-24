# CustomServerAuthenticationFilter

## Summary & Effect

It sets the customized authentication for Spring WebFlux Application.

## Example

CustomServerAuthenticationFilter.java

```java
...
import org.springframework.security.core.Authentication;
import org.springframework.security.core.context.ReactiveSecurityContextHolder;
import org.springframework.security.core.context.SecurityContext;
import org.springframework.security.core.context.SecurityContextImpl;
...

public class CustomServerAuthenticationFilter implements WebFilter {
    @Override
    public Mono<Void> filter(ServerWebExchange exchange, WebFilterChain chain) {
        return ReactiveSecurityContextHolder.getContext()
                .switchIfEmpty(Mono.defer(() -> { // When no context found
                    try {
                        // make the SecurityContext
                        var authentication = getAuthentication(exchange);
                        var securityContext = new SecurityContextImpl(authentication);
                        var securityContextMono = Mono.just(authentication);
                        
                        return chain.filter(exchange)
                                .contextWrite(ReactiveSecurityContextHolder
                                        .withSecurityContext(securityContextMono))
                                .then(Mono.empty());
                        }
                    } catch (FirebaseAuthException e) {
                        return Mono.error(e);
                    }
                }))
                .flatMap(securityContext -> chain.filter(exchange));
    }
    
    private Authentication getAuthentication(ServerWebExchange exchange) {
        // do whatever you want here
        ...
    }
}
```



WebFluxSecurityConfig.java

```java
@Configuration
public class WebFluxSecurityConfig {
    @Bean
    public SecurityWebFilterChain securityFilterChain(
        ServerHttpSecurity http,
        WebFilter authenticationWebFilter
    ) {
        ....
        
        http.addFilterBefore(
            authenticationWebFilter, 
            SecurityWebFiltersOrder.AUTHENTICATION
        );
        
        // When the role doesn't have the permission to access
        http.exceptionHandling(exception -> exception.authenticationEntryPoint(
            new HttpStatusServerEntryPoint(HttpStatus.FORBIDDEN)
        );
        
        return http.build();
    }
    
    @Bean
    public WebFilter authenticationWebFilter() {
        return new CustomServerAuthenticationWebFilter();
    }
}
```

> Ref. [AnonymousAuthenticationWebFilter](https://github.com/spring-projects/spring-security/blob/main/web/src/main/java/org/springframework/security/web/server/authentication/AnonymousAuthenticationWebFilter.java)
