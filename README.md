## Why [Aikar flags](https://aikar.co/2018/07/02/tuning-the-jvm-g1gc-garbage-collector-flags-for-minecraft/)?
Aikar studied the JVM flags for several weeks before ending with these. The G1 garbage collector offers a great stability with decent performance, but may be slow in certain situations. It helped big servers at that time and it still helps them nowadays, but Java evolved. More and more garbage collectors are being created, and a good alternative today would be ShenandoahGC, tho it drastically increases CPU usage. In a near future, ZGC will be the reference of garbage collectors for Minecraft server, but it is still in development and kills your CPU, for real. That's why I stick to the G1GC.

## Disclaimer
First and foremost, I cannot be held responsible for any problems on your server as a result of these changes. This is still experimental and bugs can happen. No one has the same configuration, everything can vary from person to person.

## Java distribution
I recommend going for [Adoptium](https://adoptium.net/), no known memory leaks and vastly used. Right now, use Java 18 as it comes with G1GC improvements over 17.

## Flags
**From 1.18, Java 17 is the minimal requirement. If you're using older versions, I recommend using Java 11. Never ever use Java 8, it's a 7 years old version and has bad performance. Newer released versions are preferred.**

***Supported server softwares:***
- [x] Vanilla
- [x] Spigot/Paper and its multiple forks – *([Mirai](https://github.com/etil2jz/Mirai) is recommended for ultimate performance)*
- [x] Sponge
- [x] Fabric
- [x] Forge

***For servers using 12GB of RAM or less:***
```java
java -Xms8G -Xmx8G -XX:+UseG1GC -XX:+ParallelRefProcEnabled -XX:MaxGCPauseMillis=200 -XX:+UnlockExperimentalVMOptions -XX:+UnlockDiagnosticVMOptions -XX:+DisableExplicitGC -XX:+AlwaysPreTouch -XX:G1NewSizePercent=30 -XX:G1MaxNewSizePercent=40 -XX:G1HeapRegionSize=8M -XX:G1ReservePercent=20 -XX:G1HeapWastePercent=5 -XX:G1MixedGCCountTarget=4 -XX:InitiatingHeapOccupancyPercent=15 -XX:G1MixedGCLiveThresholdPercent=90 -XX:G1RSetUpdatingPauseTimePercent=5 -XX:SurvivorRatio=32 -XX:+PerfDisableSharedMem -XX:MaxTenuringThreshold=1 -XX:-UseBiasedLocking -XX:UseAVX=3 -XX:+UseStringDeduplication -XX:+UseFastUnorderedTimeStamps -XX:+UseAES -XX:+UseAESIntrinsics -XX:UseSSE=4 -XX:+UseFMA -XX:AllocatePrefetchStyle=1 -XX:+UseLoopPredicate -XX:+RangeCheckElimination -XX:+EliminateLocks -XX:+DoEscapeAnalysis -XX:+UseCodeCacheFlushing -XX:+SegmentedCodeCache -XX:+UseFastJNIAccessors -XX:+OptimizeStringConcat -XX:+UseCompressedOops -XX:+UseThreadPriorities -XX:+OmitStackTraceInFastThrow -XX:+TrustFinalNonStaticFields -XX:ThreadPriorityPolicy=1 -XX:+UseInlineCaches -XX:+RewriteBytecodes -XX:+RewriteFrequentPairs -XX:+UseNUMA -XX:-DontCompileHugeMethods -XX:+UseCMoveUnconditionally -XX:+UseFPUForSpilling -XX:+UseFastStosb -XX:+UseNewLongLShift -XX:+UseVectorCmov -XX:+UseXMMForArrayCopy -XX:+UseXmmI2D -XX:+UseXmmI2F -XX:+UseXmmLoadAndClearUpper -XX:+UseXmmRegToRegMoveAll -Dfile.encoding=UTF-8 -Xlog:async --add-modules jdk.incubator.vector -jar server.jar nogui
```

***For servers using more than 12GB of RAM:***
```java
java -Xms16G -Xmx16G -XX:+UseG1GC -XX:+ParallelRefProcEnabled -XX:MaxGCPauseMillis=200 -XX:+UnlockExperimentalVMOptions -XX:+UnlockDiagnosticVMOptions -XX:+DisableExplicitGC -XX:+AlwaysPreTouch -XX:G1NewSizePercent=40 -XX:G1MaxNewSizePercent=50 -XX:G1HeapRegionSize=16M -XX:G1ReservePercent=15 -XX:G1HeapWastePercent=5 -XX:G1MixedGCCountTarget=4 -XX:InitiatingHeapOccupancyPercent=20 -XX:G1MixedGCLiveThresholdPercent=90 -XX:G1RSetUpdatingPauseTimePercent=5 -XX:SurvivorRatio=32 -XX:+PerfDisableSharedMem -XX:MaxTenuringThreshold=1 -XX:-UseBiasedLocking -XX:UseAVX=3 -XX:+UseStringDeduplication -XX:+UseFastUnorderedTimeStamps -XX:+UseAES -XX:+UseAESIntrinsics -XX:UseSSE=4 -XX:+UseFMA -XX:AllocatePrefetchStyle=1 -XX:+UseLoopPredicate -XX:+RangeCheckElimination -XX:+EliminateLocks -XX:+DoEscapeAnalysis -XX:+UseCodeCacheFlushing -XX:+SegmentedCodeCache -XX:+UseFastJNIAccessors -XX:+OptimizeStringConcat -XX:+UseCompressedOops -XX:+UseThreadPriorities -XX:+OmitStackTraceInFastThrow -XX:+TrustFinalNonStaticFields -XX:ThreadPriorityPolicy=1 -XX:+UseInlineCaches -XX:+RewriteBytecodes -XX:+RewriteFrequentPairs -XX:+UseNUMA -XX:-DontCompileHugeMethods -XX:+UseCMoveUnconditionally -XX:+UseFPUForSpilling -XX:+UseFastStosb -XX:+UseNewLongLShift -XX:+UseVectorCmov -XX:+UseXMMForArrayCopy -XX:+UseXmmI2D -XX:+UseXmmI2F -XX:+UseXmmLoadAndClearUpper -XX:+UseXmmRegToRegMoveAll -Dfile.encoding=UTF-8 -Xlog:async --add-modules jdk.incubator.vector -jar server.jar nogui
```

*of course, adjust -Xms and -Xmx values to whatever you want*

*same for your java path and server jar*

## Sources
[GraalVM release notes](https://www.graalvm.org/release-notes/)
