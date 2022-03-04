# [Intellij] finished with non-zero exit value 1 오류



Intellij, Gradle 환경에서 프로젝트 첫 실행 시 아래와 같은 오류가 발생

```
Execution failed for task ':Application.main()'.
> Process 'command 'JDK경로/bin/java.exe' finished with non-zero exit value 1

* Try:
Run with --stacktrace option to get the stack trace. Run with --info or --debug option to get more log output. Run with --scan to get full insights.
```



## 해결방법

1. [File > Settings] 메뉴 클릭 (단축키 : ctrl + alt + s)
2. [Build, Execution, Deployment > Build Tools > Gradle] 메뉴로 이동
3. Build and run using 을 IntelliJ IDEA로 변경
4. Run tests using 을 IntelliJ IDEA로 변경
5. Gradle JVM을 jdk11로 변경 (없다면 설치)