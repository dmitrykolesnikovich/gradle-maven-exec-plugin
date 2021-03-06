# Frequently Asked Questions

- [Cannot convert relative path . to an absolute file](#i'm-getting-errors-like-'cannot-convert-relative-path-.-to-an-absolute-file.')
- [MavenExecAction.setWorkingDir(Ljava/io/File;)V](#i'm-getting-mavenexecaction.setworkingdir(ljava/io/file;)v)

## I'm getting errors like 'Cannot convert relative path . to an absolute file.'

You need to specify the working directory for the plugin. Most often it will be
```groovy
mavenexec {
    workingDir projectDir
}
```

## I'm getting MavenExecAction.setWorkingDir(Ljava/io/File;)V
##### A problem occurred evaluating root project '...'. com.github.dkorotych.gradle.maven.exec.MavenExecAction.setWorkingDir(Ljava/io/File;)V

In Gradle, starting with version 4, was added an additional method to set the working directory, which takes a File object as a parameter. And if you specify a working directory as a file object, then the plugin can not correctly recognize which of the real methods it needs to call and tries to call an abstract method, which is suitable for the required signature. To avoid this error, simply change the object type to set the working directory. For example, a construction of the form
```groovy
mavenexec {
    workingDir = projectDir
}
```
suffices to change to
```groovy
mavenexec {
    workingDir projectDir
}
```
or on
```groovy
mavenexec {
    workingDir = projectDir.toURI()
}
```
