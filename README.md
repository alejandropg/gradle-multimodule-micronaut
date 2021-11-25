# gradle-multimodule-micronaut

This is a simple example of a multi module Gradle proyect with Micronaut 3.2.0

This exmample use the new `micronaut-gradle-plugin` version `3.0.0`

In this example, if you try to use dependency `io.micronaut.test:micronaut-test-junit5` and the plugin `io.micronaut.application`, the build fails with the message:

```
FAILURE: Build failed with an exception.

* What went wrong:
A problem occurred configuring project ':app'.
> The value for this property is final and cannot be changed any further.
```

You can reproduce the problem with

```bash
gradle build
```

And see the complete stacktrace with:

```bash
gradle build --stacktrace
```

The problem looks like in file `MicronautLibraryPlugin` lines 186 and 184, when the plugins try to detect the testing framework and apply it twice.

```
Caused by: java.lang.IllegalStateException: The value for this property is final and cannot be changed any further.
        at org.gradle.api.internal.provider.AbstractProperty$FinalizedValue.beforeMutate(AbstractProperty.java:489)
        at org.gradle.api.internal.provider.AbstractProperty.assertCanMutate(AbstractProperty.java:263)
        at org.gradle.api.internal.provider.AbstractProperty.setSupplier(AbstractProperty.java:212)
        at org.gradle.api.internal.provider.DefaultProperty.set(DefaultProperty.java:71)
        at org.gradle.api.tasks.testing.Test.useTestFramework(Test.java:979)
        at org.gradle.api.tasks.testing.Test.useJUnitPlatform(Test.java:1049)
        at org.gradle.api.tasks.testing.Test.useJUnitPlatform(Test.java:1032)
----->  at io.micronaut.gradle.MicronautLibraryPlugin.lambda$null$3(MicronautLibraryPlugin.java:186)
        at org.gradle.configuration.internal.DefaultUserCodeApplicationContext$CurrentApplication$1.execute(DefaultUserCodeApplicationContext.java:123)
        at org.gradle.api.internal.DefaultCollectionCallbackActionDecorator$BuildOperationEmittingAction$1.run(DefaultCollectionCallbackActionDecorator.java:110)
        at org.gradle.internal.operations.DefaultBuildOperationRunner$1.execute(DefaultBuildOperationRunner.java:29)
        at org.gradle.internal.operations.DefaultBuildOperationRunner$1.execute(DefaultBuildOperationRunner.java:26)
        at org.gradle.internal.operations.DefaultBuildOperationRunner$2.execute(DefaultBuildOperationRunner.java:66)
        at org.gradle.internal.operations.DefaultBuildOperationRunner$2.execute(DefaultBuildOperationRunner.java:59)
        at org.gradle.internal.operations.DefaultBuildOperationRunner.execute(DefaultBuildOperationRunner.java:157)
        at org.gradle.internal.operations.DefaultBuildOperationRunner.execute(DefaultBuildOperationRunner.java:59)
        at org.gradle.internal.operations.DefaultBuildOperationRunner.run(DefaultBuildOperationRunner.java:47)
        at org.gradle.internal.operations.DefaultBuildOperationExecutor.run(DefaultBuildOperationExecutor.java:68)
        at org.gradle.api.internal.DefaultCollectionCallbackActionDecorator$BuildOperationEmittingAction.execute(DefaultCollectionCallbackActionDecorator.java:107)
        at org.gradle.api.internal.collections.CollectionFilter$1.execute(CollectionFilter.java:59)
        at org.gradle.api.internal.DefaultDomainObjectCollection.all(DefaultDomainObjectCollection.java:159)
        at org.gradle.api.internal.tasks.DefaultRealizableTaskCollection.all(DefaultRealizableTaskCollection.java:224)
        at org.gradle.api.internal.DefaultDomainObjectCollection.withType(DefaultDomainObjectCollection.java:201)
----->  at io.micronaut.gradle.MicronautLibraryPlugin.lambda$apply$4(MicronautLibraryPlugin.java:184)
        at org.gradle.configuration.internal.DefaultUserCodeApplicationContext$CurrentApplication$1.execute(DefaultUserCodeApplicationContext.java:123)
        at org.gradle.configuration.internal.DefaultListenerBuildOperationDecorator$BuildOperationEmittingAction$1.run(DefaultListenerBuildOperationDecorator.java:152)
        at org.gradle.internal.operations.DefaultBuildOperationRunner$1.execute(DefaultBuildOperationRunner.java:29)
        at org.gradle.internal.operations.DefaultBuildOperationRunner$1.execute(DefaultBuildOperationRunner.java:26)
```
