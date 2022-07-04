# Parchment 简述与使用

[有条件的可以移步 GitHub 看原文](https://github.com/ParchmentMC/Librarian/blob/dev/docs/FORGEGRADLE.md)

简单来说，Parchment 是一个参数表，用来补足官方表的参数。

听不懂没关系，只需要知道他是可以帮你解决下面这种情况的工具就可以。

```java
public Block(Properties p_49795_)
```

## 使用方法

修改 build.gradle 文件

```groovy
buildscript {
    repositories {
        maven { url = 'https://maven.minecraftforge.net' }
        // 需要你添加的内容
        maven { url = 'https://maven.parchmentmc.org' }
        ...
    }
    dependencies {
        classpath group: 'net.minecraftforge.gradle', name: 'ForgeGradle', version: '5.1.+', changing: true
        // 需要你添加的内容
        classpath 'org.parchmentmc:librarian:1.+'
    ...
...
apply plugin: 'net.minecraftforge.gradle'
// 需要你添加的内容
apply plugin: 'org.parchmentmc.librarian.forgegradle'
...
minecraft {
    // 原mapping mappings channel: 'official', version: '1.18.2'
    // 需要你添加的内容
    mappings channel: 'parchment', version: '2022.03.13-1.18.2'
```
然后重新加载 gradle 项目即可。