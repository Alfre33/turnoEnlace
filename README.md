# TurnoEnlace – Estrategia de Ramas (GitLab Flow con entornos)

Este repositorio usa **GitLab Flow (ramas de entorno)** aplicado en GitHub:
- Ramas persistentes: `develop` → `uat` → `main`
- Promoción por Pull Request (PR), con CI verde y aprobaciones
- Despliegues por entorno usando **GitHub Environments**

## Ramas y propósito
- `develop`: Integración diaria. Cada merge despliega a entorno **develop**.
- `uat`: Validación con negocio/QA. Promoción desde `develop`.
- `main`: Producción. Promoción desde `uat` o hotfix urgente.

## Ramas de trabajo (cortas)
- `feature/<slug>`: nueva funcionalidad (**nacen desde `develop`**)
- `bugfix/<slug>`: corrección (desde `develop` o `uat`, según dónde se detectó)
- `hotfix/<slug>`: corrección crítica en producción (desde `main`)

## Flujo de promoción
1. Merge de `feature/*` → `develop` (CI verde + 2 approvals)
2. PR **develop → uat** (“Promote to UAT”) (CI + approvals + deploy a UAT)
3. PR **uat → main** (“Promote to main) (CI + approvals + deploy a main)

**Hotfix**: `hotfix/*` → PR a `main` → luego back-merge a `uat` y `develop`.

## Reglas de contribución
- **Prohibido** hacer push directo a `develop`, `uat`, `main`.
- Todo cambio entra por **PR** con:
  - **2 aprobaciones** obligatorias
  - **Status checks** en verde (lint/test/build)
  - (Opcional) **Code Owners** deben aprobar si aplica
- Habilitado “dismiss stale approvals” (si cambias el PR, se revalida).

## Convenciones
- Branch naming: `feature/*`, `bugfix/*`, `hotfix/*`, `release/*`
- Commits: Conventional Commits (ej. `feat(auth): enable 2FA`)
- Versionado: **SemVer** (`vX.Y.Z`) y **tags protegidos** (`v*`)

## CI/CD
- CI en PRs y en `develop` (`.github/workflows/ci.yml`)
- Deploy automático en `develop` al merge
- Deploy a `uat`/`main` al merge en sus ramas (environments con approvals)
- Scripts de despliegue: `scripts/deploy-*.sh`

## Environments (GitHub)
- `develop`: sin aprobaciones manuales
- `uat`: con **required reviewers**
- `main(production)`: con **required reviewers** + auditoría de despliegue

## Roles
- Mantenedores: administran reglas y approvals finales de `main`
- Equipo de QA/Negocio: aprueba PRs de promoción a `uat`
- Equipo de Ingeniería: revisa PRs en `develop` y features

