
# GitHub Actions + Maven – Automatyczne wersjonowanie (SNAPSHOT & RELEASE)

Przykładowe repozytorium pokazujące **jak poprawnie i zgodnie ze sztuką zarządzać wersjami w projektach Maven przy użyciu GitHub Actions**.

Repo demonstruje **pełny, produkcyjny workflow**:
- `develop` → wersje **SNAPSHOT**
- `master` → wersje **RELEASE**
- automatyczne **podbijanie wersji**
- **commit + tag + deploy**
- brak ręcznej edycji `pom.xml`

Ten wzorzec jest powszechnie stosowany w projektach enterprise z Nexus / Artifactory / Reposilite.

---

## Najważniejsze założenia

- Jedno źródło prawdy o wersji: **Git**
- Każda zmiana wersji:
  - jest commitowana
  - jest audytowalna
  - może być cofnięta
- Brak ręcznych zmian wersji w `pom.xml`

---

## Strategia branchy

### develop
- zawsze **x.y.z-SNAPSHOT**
- po każdym pushu:
  1. build + test
  2. bump patch
  3. commit do repo
  4. deploy do repo snapshots

### master
- tylko wersje release
- po pushu:
  1. bump minor
  2. commit + tag
  3. deploy release
  4. bump develop do `release + patch + SNAPSHOT`

---

## Przykładowy flow

```
develop: 0.0.26-SNAPSHOT
master:  0.1.0
develop: 0.1.1-SNAPSHOT
```

---

## Maven distributionManagement

```xml
<distributionManagement>
  <repository>
    <id>reposilite-releases</id>
    <url>https://your-host/repository/releases</url>
  </repository>
  <snapshotRepository>
    <id>reposilite-snapshots</id>
    <url>https://your-host/repository/snapshots</url>
  </snapshotRepository>
</distributionManagement>
```

---

## settings.xml

```xml
<settings>
  <servers>
    <server>
      <id>releases</id>
      <username>${env.MAVEN_REPO_USER}</username>
      <password>${env.MAVEN_REPO_PASSWORD}</password>
    </server>
    <server>
      <id>snapshots</id>
      <username>${env.MAVEN_REPO_USER}</username>
      <password>${env.MAVEN_REPO_PASSWORD}</password>
    </server>
  </servers>
</settings>
```

---

## GitHub Secrets

- MAVEN_USER
- MAVEN_PASS

Mapowane na zmienne środowiskowe w workflow.



MIT
