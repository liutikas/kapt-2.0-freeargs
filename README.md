# kapt task failing when serializing configuration cache

1. `./gradlew assembleDebug`

## Expected
Success

## Actual
Failure when serializing Gradle configuration cache
```
Calculating task graph as no cached configuration is available for tasks: assembleDebug

FAILURE: Build failed with an exception.

* What went wrong:
Configuration cache state could not be cached: field `disableMultiModuleIC$delegate` of task `:app:kaptGenerateStubsDebugKotlin` of type `org.jetbrains.kotlin.gradle.internal.KaptGenerateStubsTask`: error writing value of type 'kotlin.SynchronizedLazyImpl'
> Cannot query the value of property 'freeCompilerArgs' because it has no value available.

```

## Sidenotes

Moving `tasks.withType(KotlinCompiler::class.java)` out of `afterEvaluate {}`
makes it no longer fail, but we do want `afterEvaluate` to read values after
Gradle plugin Extensions are evaluated.
