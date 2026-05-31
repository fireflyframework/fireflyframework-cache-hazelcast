# Firefly Framework - Cache - Hazelcast

[![CI](https://github.com/fireflyframework/fireflyframework-cache-hazelcast/actions/workflows/ci.yml/badge.svg)](https://github.com/fireflyframework/fireflyframework-cache-hazelcast/actions/workflows/ci.yml)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)
[![Java](https://img.shields.io/badge/Java-21%2B-orange.svg)](https://openjdk.org)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.x-green.svg)](https://spring.io/projects/spring-boot)

> Hazelcast distributed cache provider for the Firefly Framework cache abstraction, discovered via the JDK ServiceLoader SPI.

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Requirements](#requirements)
- [Installation](#installation)
- [Quick Start](#quick-start)
- [Configuration](#configuration)
- [Documentation](#documentation)
- [Contributing](#contributing)
- [License](#license)

## Overview

Firefly Framework Cache Hazelcast plugs a Hazelcast-backed `CacheAdapter` into the unified `fireflyframework-cache` abstraction. It is discovered purely through the JDK `ServiceLoader` SPI: dropping this jar on the classpath registers a `HazelcastProvider` (a `CacheProviderFactory` reporting `CacheType.HAZELCAST`) that the core `CacheManagerFactory` can select.

The provider is SPI-only — it ships **no** Spring auto-configuration. Instead it binds to a `com.hazelcast.core.HazelcastInstance` bean that the host application supplies. The adapter itself (`HazelcastCacheHelper`) accesses Hazelcast reflectively, so the core cache module never carries a compile-time Hazelcast dependency.

## Features

- `HazelcastProvider` registered via `META-INF/services/org.fireflyframework.cache.spi.CacheProviderFactory`
- Backs the cache abstraction with a Hazelcast `IMap`, including TTL-aware writes
- Reflective adapter (`HazelcastCacheHelper`) that keeps the core module Hazelcast-free
- Reports `CacheType.HAZELCAST` with a provider priority of 20
- Honors the configured `keyPrefix` (`<prefix>:<cacheName>:`) for key namespacing
- No Spring auto-configuration required — binds to a host-supplied `HazelcastInstance` bean

## Requirements

- Java 21+
- Spring Boot 3.x
- Maven 3.9+
- A `com.hazelcast.core.HazelcastInstance` bean provided by the host application

## Installation

```xml
<dependency>
    <groupId>org.fireflyframework</groupId>
    <artifactId>fireflyframework-cache-hazelcast</artifactId>
    <version>26.05.07</version>
</dependency>
```

## Quick Start

```xml
<dependencies>
    <dependency>
        <groupId>org.fireflyframework</groupId>
        <artifactId>fireflyframework-cache</artifactId>
    </dependency>
    <dependency>
        <groupId>org.fireflyframework</groupId>
        <artifactId>fireflyframework-cache-hazelcast</artifactId>
    </dependency>
</dependencies>
```

The host application must supply a `HazelcastInstance` bean and select Hazelcast as the cache type:

```java
@Bean
public HazelcastInstance hazelcastInstance() {
    return Hazelcast.newHazelcastInstance();
}
```

## Configuration

The host application must expose a `com.hazelcast.core.HazelcastInstance` bean and set the default cache type to `HAZELCAST`:

```yaml
firefly:
  cache:
    default-cache-type: HAZELCAST
```

Once both the `HazelcastInstance` bean and this property are present, the core `CacheManagerFactory` discovers the `HazelcastProvider` through the SPI and routes caching to Hazelcast.

## Documentation

No additional documentation available for this project.

## Contributing

Contributions are welcome. Please read the [CONTRIBUTING.md](CONTRIBUTING.md) guide for details on our code of conduct, development process, and how to submit pull requests.

## License

Copyright 2024-2026 Firefly Software Foundation.

Licensed under the Apache License, Version 2.0. See [LICENSE](LICENSE) for details.
