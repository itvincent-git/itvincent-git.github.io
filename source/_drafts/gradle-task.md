Gradle task时发现一个问题

task里的代码不是在正常的task运行时运行，而是在gradle一开始的时候运行



解决方案：

用doLast{}封装了代码，这样才会在task运行时才跑