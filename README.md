## Ferramentas de segurança rodando em Pipelines de Integração Contínua

### Requisitos:
- <a href="https://www.docker.com/">Docker</a>
- <a href="https://docs.docker.com/compose/">Docker Compose</a>

### Plugins no Jenkins:
- HTML Publisher

### Plugins no Sonar:
- Dependency-Check

### Dependency Check install script:
```
if [ ! -f /tmp/dependency-check/bin/dependency-check.sh ]
then
  curl -Lo /tmp/dependency-check.zip https://github.com/jeremylong/DependencyCheck/releases/download/v6.5.3/dependency-check-6.5.3-release.zip
  unzip /tmp/dependency-check.zip -d /tmp/
fi
/tmp/dependency-check/bin/dependency-check.sh -s . --disableYarnAudit -project NodeGoat -o . -f ALL
```

### SonarScanner install script:
```
if [ ! -f /tmp/sonar-scanner/bin/sonar-scanner ]
then
  curl -Lo /tmp/sonarscanner.zip https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-4.6.2.2472-linux.zip
  unzip /tmp/sonarscanner.zip -d /tmp
fi
/tmp/sonar-scanner/bin/sonar-scanner \
  -Dsonar.projectKey=NodeGoat \
  -Dsonar.sources=. \
  -Dsonar.dependencyCheck.jsonReportPath=dependency-check-report.json \
  -Dsonar.dependencyCheck.htmlReportPath=dependency-check-report.html \
  -Dsonar.host.url=http://172.1.1.20:9000 \
  -Dsonar.login=<login_key>
```