/**Segun clase hernan */ 

/**
     * Configuración global para todos los circuit breakers que no tengan una configuración específica.
     * <p>
     * La configuración define un umbral del 50% de fallas antes de abrir el circuito,
     * con una espera de 5 segundos en estado abierto y una ventana deslizante de 10 intentos.
     * También limita las operaciones a un tiempo máximo de 2 segundos.
     * </p>
     *
     * @return Customizer para configurar el comportamiento predeterminado de los circuit breakers.
     */

   ### Ejemplo
   ```java
   @Configuration
    public class Resilience4JConfiguration {  
        
        @Bean
        public Customizer<Resilience4JCircuitBreakerFactory> globalCustomConfiguration() {
        // Configura el Circuit Breaker con un umbral de fallo del 50% y una ventana de 10 intentos.
        CircuitBreakerConfig circuitBreakerConfig = CircuitBreakerConfig.custom()
                .failureRateThreshold(50)  // Umbral del 50% para considerar fallas.
                .waitDurationInOpenState(Duration.ofSeconds(5))  // Espera de 5 segundos en estado abierto.
                .slidingWindowSize(10)  // Tamaño de la ventana deslizante.
                .build();

        // Configura un límite de tiempo para las operaciones (máximo 2 segundos).
        TimeLimiterConfig timeLimiterConfig = TimeLimiterConfig.custom()
                .timeoutDuration(Duration.ofSeconds(2))  // Limita las operaciones a 2 segundos.
                .build();

        // Retorna la configuración aplicada por defecto a todos los circuitos
        return factory -> factory.configureDefault(id -> new Resilience4JConfigBuilder(id)
                .timeLimiterConfig(timeLimiterConfig)
                .circuitBreakerConfig(circuitBreakerConfig)
                .build());
        }
    }