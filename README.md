# TurnoEnlace – Estrategia de Ramas (GitLab Flow con entornos)

Este repositorio usa **GitLab Flow (ramas de entorno)** aplicado en GitHub:
- Ramas persistentes: `dev` → `uat` → `main`
- Promoción por Pull Request (PR), con CI verde y aprobaciones
- Despliegues por entorno usando **GitHub Environments**

## Ramas y propósito
- `dev`: Integración diaria. Cada merge despliega a entorno **dev**.
- `uat`: Validación con negocio/QA. Promoción desde `dev`.
- `main`: Producción. Promoción desde `uat` o hotfix urgente.

## Ramas de trabajo (cortas)
- `feature/<slug>`: nueva funcionalidad (**nacen desde `dev`**)
- `bugfix/<slug>`: corrección (desde `dev` o `uat`, según dónde se detectó)
- `hotfix/<slug>`: corrección crítica en producción (desde `main`)

## Flujo de promoción
1. Merge de `feature/*` → `dev` (CI verde + 2 approvals)
2. PR **dev → uat** (“Promote to UAT”) (CI + approvals + deploy a UAT)
3. PR **uat → prod** (“Promote to Prod”) (CI + approvals + deploy a Prod)

**Hotfix**: `hotfix/*` → PR a `main` → luego back-merge a `uat` y `dev`.

## Reglas de contribución
- **Prohibido** hacer push directo a `dev`, `uat`, `main`.
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
- CI en PRs y en `dev` (`.github/workflows/ci.yml`)
- Deploy automático en `dev` al merge
- Deploy a `uat`/`main` al merge en sus ramas (environments con approvals)
- Scripts de despliegue: `scripts/deploy-*.sh`

## Environments (GitHub)
- `dev`: sin aprobaciones manuales
- `uat`: con **required reviewers**
- `main(production)`: con **required reviewers** + auditoría de despliegue

## Roles
- Mantenedores: administran reglas y approvals finales de `main`
- Equipo de QA/Negocio: aprueba PRs de promoción a `uat`
- Equipo de Ingeniería: revisa PRs en `dev` y features

