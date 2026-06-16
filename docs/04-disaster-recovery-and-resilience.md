# 04-disaster-recovery-and-resilience

## Objetivo

Definir estrategia de recuperacion ante desastres y resiliencia para servicios transaccionales Tier 1 en AWS.

## Objetivos de continuidad Tier 1

- RTO objetivo: menor a 15 minutos.
- RPO objetivo: menor a 1 minuto.
- Disponibilidad operativa sostenida bajo degradaciones parciales.

## Estrategia DR recomendada

Enfoque principal: Multi-Region Active-Passive con modelo Pilot Light.

- Region primaria activa para trafico productivo.
- Region secundaria con componentes base preprovisionados y datos replicados.
- Activacion controlada de capacidad completa en region secundaria ante desastre.
- Runbooks automatizados para failover y restauracion de rutas.

Razonamiento:
- Reduce costo frente a Active-Active pleno.
- Mantiene tiempos de recuperacion compatibles con objetivos de pagos criticos.
- Permite evolucion gradual hacia Active-Active para dominios de mayor criticidad.

## Patrones de resiliencia en plataforma

- Circuit Breaker: evita cascadas de fallos en integraciones externas.
- Retries con Exponential Backoff: controla reintentos para no saturar dependencias.
- SQS Dead-Letter Queues: preserva eventos fallidos para reproceso controlado.
- Timeout y bulkhead por componente para aislar fallas.
- Health checks y deployment rollback automatico ante degradacion.

## Operacion y pruebas

- Game days trimestrales para validar runbooks DR.
- Pruebas de failover por dominio critico con evidencia de tiempos reales.
- Seguimiento de brecha entre objetivo y resultado de RTO/RPO.
- Ajuste de arquitectura segun hallazgos de incidentes y simulacros.

## Relacionado

- [01 - Architecture Overview](01-architecture-overview.md)
- [02 - Infrastructure as Code](02-infrastructure-as-code.md)
- [03 - Security and Compliance](03-security-and-compliance.md)
- [05 - Cost Optimization and FinOps](05-cost-optimization-finops.md)
- Referencia de governance CI/CD del portfolio: [enterprise-cicd-governance-blueprint](https://github.com/andminin-engineering/enterprise-cicd-governance-blueprint)
