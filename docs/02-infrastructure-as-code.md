# 02-infrastructure-as-code

## Objetivo

Definir gobernanza Terraform enterprise para una plataforma AWS multi-ambiente con control de cambios auditable y despliegues de infraestructura seguros.

## Estructura recomendada de proyecto Terraform

```text
terraform/
  modules/
    network/
    ecs-service/
    aurora-postgresql/
    elasticache-redis/
    security-baseline/
    observability-baseline/
  environments/
    dev/
      region-primary/
        main.tf
        variables.tf
        outputs.tf
    staging/
      region-primary/
        main.tf
        variables.tf
        outputs.tf
    prod/
      region-primary/
        main.tf
        variables.tf
        outputs.tf
      region-dr/
        main.tf
        variables.tf
        outputs.tf
  policies/
    sentinel-or-opa/
  ci/
    terraform-workflow-guidelines.md
```

## Estado remoto y locking

- Backend remoto en S3 por ambiente y region.
- Locking de estado con DynamoDB para evitar escrituras concurrentes.
- Versionado y cifrado de buckets de estado habilitado por defecto.
- Politica de acceso minimo al backend solo para pipelines autorizados y platform team.

## CI/CD para infraestructura

- Pull Request con speculative plan obligatorio antes de merge.
- Merge a main habilita plan completo y aprobacion manual para apply en ambientes criticos.
- Promotion secuencial dev -> staging -> prod con evidencia de plan/apply por cada etapa.
- Rollback de infraestructura controlado por versionado de modulos y estado.

## Testing y seguridad para IaC

- Validacion de sintaxis y formato en cada cambio.
- TFLint para estandares y anti-patterns de Terraform.
- Checkov para controles de seguridad y compliance cloud.
- Politicas como codigo para bloquear configuraciones no permitidas.
- Drift detection recurrente para detectar cambios fuera de Terraform.

## Modelo de gobernanza

- Catalogo de modulos reutilizables con versionado semantico.
- Architecture Board aprueba cambios estructurales de modulos base.
- Excepciones solo con ticket, justificacion de negocio y fecha de vencimiento.
- Revisiones trimestrales de politicas de IaC por riesgo y costo.

## Relacionado

- [01 - Architecture Overview](01-architecture-overview.md)
- [03 - Security and Compliance](03-security-and-compliance.md)
- [04 - Disaster Recovery and Resilience](04-disaster-recovery-and-resilience.md)
- [05 - Cost Optimization and FinOps](05-cost-optimization-finops.md)
- Referencia de governance CI/CD del portfolio: [enterprise-cicd-governance-blueprint](https://github.com/andminin-engineering/enterprise-cicd-governance-blueprint)
