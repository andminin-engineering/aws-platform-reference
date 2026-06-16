# 03-security-and-compliance

## Objetivo

Establecer el pilar de seguridad y cumplimiento para una plataforma de pagos en AWS bajo principios Zero-Trust y alineacion con controles PCI-DSS.

## Modelo Zero-Trust

- Verificacion explicita de identidad de usuarios, servicios y workloads.
- Acceso minimo necesario por contexto, rol y tiempo.
- Segmentacion de red para aislar dominios de aplicacion y datos.
- Telemetria continua para deteccion temprana de anomalias.

## IAM y minimo privilegio

- Roles por servicio con permisos acotados a recursos especificos.
- Eliminacion de credenciales estaticas y uso de identidades temporales.
- Politicas administradas por dominio y revisadas por seguridad.
- Control de permisos sensibles con aprobacion dual.

## Gestion de secretos

- AWS Secrets Manager para credenciales de base de datos, API keys y tokens.
- Rotacion automatica de secretos segun criticidad de servicio.
- Prohibicion de secretos en variables de build sin cifrado.
- Auditoria de acceso y uso de secretos por ambiente.

## Cifrado y proteccion de datos

- En reposo: cifrado con AWS KMS para Aurora, S3, EBS y secretos.
- En transito: TLS 1.3 extremo a extremo en trafico interno y externo.
- Politicas de llaves con separacion de deberes y rotacion programada.

## Proteccion perimetral

- AWS WAF para filtrado de trafico malicioso y reglas de aplicacion.
- AWS Shield para mitigacion de ataques DDoS.
- Integracion de controles edge con alertas y runbooks operativos.

## Mapeo con PCI-DSS

- Segmentacion de red para aislar componentes del Cardholder Data Environment.
- Trazabilidad de accesos administrativos y tecnicos.
- Endurecimiento de configuraciones base y gestion de vulnerabilidades.
- Evidencia auditable de cambios de infraestructura y despliegues.

## Relacionado

- [01 - Architecture Overview](01-architecture-overview.md)
- [02 - Infrastructure as Code](02-infrastructure-as-code.md)
- [04 - Disaster Recovery and Resilience](04-disaster-recovery-and-resilience.md)
- [05 - Cost Optimization and FinOps](05-cost-optimization-finops.md)
- Referencia de governance CI/CD del portfolio: [enterprise-cicd-governance-blueprint](https://github.com/andminin-engineering/enterprise-cicd-governance-blueprint)
