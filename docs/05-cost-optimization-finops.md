# 05-cost-optimization-finops

## Objetivo

Definir una estrategia FinOps para controlar costo cloud sin comprometer disponibilidad, seguridad ni performance de la plataforma de pagos.

## Principios FinOps para plataforma transaccional

- Gasto alineado a valor de negocio y criticidad por tier.
- Elasticidad inteligente basada en demanda real de transacciones.
- Transparencia total por dominio, equipo y producto.
- Decision de costo con contexto de riesgo operativo.

## Mecanismos de optimizacion

### Auto-scaling por metricas de negocio

- Escalado de servicios ECS basado en TPS, latencia p95 y cola de trabajo.
- Politicas diferenciadas por horario y estacionalidad comercial.
- Umbrales de escalado validados con pruebas de carga.

### Fargate Spot para no productivo

- Uso de Fargate Spot en dev y staging para cargas no criticas.
- Exclusion de workloads Tier 1 productivos de capacidad interrumpible.
- Estrategia mixta para balancear costo y confiabilidad.

### Ciclo de vida S3

- Politicas de lifecycle para transicionar datos historicos a clases de menor costo.
- Retencion regulatoria separada de datos operativos.
- Limpieza automatizada de artefactos temporales y logs no requeridos.

### Compute Savings Plans

- Cobertura de consumo base estable de servicios productivos.
- Revision mensual de utilization y commitment.
- Ajuste progresivo de compromisos segun crecimiento de demanda.

### Cost Allocation Tags obligatorios

- Etiquetas estandar por producto, dominio, ambiente, owner y criticidad.
- Bloqueo de despliegues sin etiquetado minimo.
- Dashboards de costo por unidad de negocio y por capacidad tecnica.

## Gobierno y seguimiento

- Revisiones FinOps quincenales entre plataforma y negocio.
- Alertas de desviacion presupuestaria por ambiente.
- KPIs: costo por transaccion, utilization de compute, ahorro por optimizacion.
- Backlog de optimizacion con impacto esperado y fecha objetivo.

## Relacionado

- [01 - Architecture Overview](01-architecture-overview.md)
- [02 - Infrastructure as Code](02-infrastructure-as-code.md)
- [03 - Security and Compliance](03-security-and-compliance.md)
- [04 - Disaster Recovery and Resilience](04-disaster-recovery-and-resilience.md)
- Referencia de governance CI/CD del portfolio: [enterprise-cicd-governance-blueprint](https://github.com/andminin-engineering/enterprise-cicd-governance-blueprint)
