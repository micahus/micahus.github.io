---
title: "Springboot问题汇总"
date: 2023-10-14T21:43:48+08:00
tags: ["java"]
categories: ["Learning", "Reflections"]
---

### 1. 编写starter时，引用后无法获取到bean

> 环境：使用版本Springboot3.1.4版本

问题：之前写starter的时候，一直是按照网上的教程，大部分是Springboot2.x版本，但自己使用了最新版本Springboot3.1.4版本，所以一直不成功。

深入代码查看后，发现以下跟老版本不一致。

新版本：

```
SpringBootApplication --> EnableAutoConfiguration --> AutoConfigurationImportSelector.getCandidateConfigurations --> ImportCandidates.load --> LOCATION --> "META-INF/spring/%s.imports"
```

老版本：

```
SpringBootApplication --> EnableAutoConfiguration --> AutoConfigurationImportSelector.getCandidateConfigurations --> SpringFactoriesLoader.loadFactoryNames --> Factories_RESOURCE_LOCATION --> "META-INF/spring.factories"
```

所以按照新版 ` META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports` 进行编写的，可以正常识别。

注意：

这里的内容也不一样了，之前是需要写入一行，新版本支持多行格式

```java
		Enumeration<URL> urls = findUrlsInClasspath(classLoaderToUse, location);
		List<String> importCandidates = new ArrayList<>();
		while (urls.hasMoreElements()) { // 如果有多行，直接支持了，把所有的都加入到importCandidates中去了
			URL url = urls.nextElement();
			importCandidates.addAll(readCandidateConfigurations(url));
		}
```

