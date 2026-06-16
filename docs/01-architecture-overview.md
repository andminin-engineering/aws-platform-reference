# 01-architecture-overview

## Objetivo

Definir una arquitectura de referencia en AWS para una plataforma de pagos transaccional de alta criticidad, con foco en disponibilidad, latencia y cumplimiento.

## C4 Context

```mermaid
flowchart LR
    Customer[Cliente Final] --> Merchant[Comercio]
    Merchant --> Platform[Payment Platform]
    Platform --> Acquirer[Adquirente]
    Platform --> Issuer[Emisor]
    Platform --> Fraud[Servicio Antifraude]
    Platform --> Ops[Operacion y Soporte]
    Regulator[Regulador y Compliance] --> Platform
```

## Arquitectura AWS detallada

```mermaid
flowchart TB
    subgraph Edge[Edge y Seguridad]
        CF[Amazon CloudFront]
        WAF[AWS WAF]
        SH[AWS Shield]
    end

    subgraph VPC[VPC Multi-AZ]
        subgraph PublicAZA[Public Subnet AZ-A]
            ALB1[Application Load Balancer]
        end
        subgraph PublicAZB[Public Subnet AZ-B]
            ALB2[Application Load Balancer]
        end

        subgraph PrivateAZA[Private App Subnet AZ-A]
            ECSA[ECS Fargate Tasks\nJava Spring Boot / Node.js NestJS]
        end
        subgraph PrivateAZB[Private App Subnet AZ-B]
            ECSB[ECS Fargate Tasks\nJava Spring Boot / Node.js NestJS]
        end

        subgraph DataLayer[Private Data Subnets]
            AURW[Aurora PostgreSQL Writer]
            AURR[Aurora Read Replicas]
            REDIS[ElastiCache Redis\nTokens/Sesiones]
        end

        subgraph Integration[Conectividad Privada]
            PL[AWS PrivateLink\nIntegraciones externas seguras]
        end
    end

    CF --> WAF
    WAF --> SH
    SH --> ALB1
    SH --> ALB2
    ALB1 --> ECSA
    ALB2 --> ECSB
    ECSA --> AURW
    ECSB --> AURW
    ECSA --> AURR
    ECSB --> AURR
    ECSA --> REDIS
    ECSB --> REDIS
    ECSA --> PL
    ECSB --> PL
```

## Decisiones clave

- CloudFront se usa para reducir latencia de entrada, proteger el edge y centralizar politicas.
- ALB distribuye trafico HTTP/HTTPS con health checks hacia tareas ECS en subredes privadas.
- ECS Fargate desacopla operacion de contenedores de la gestion de nodos.
- Aurora PostgreSQL ofrece alta disponibilidad en Multi-AZ y replicas para lectura.
- ElastiCache Redis reduce latencia para tokens, sesiones y datos de acceso frecuente.
- PrivateLink limita exposicion de integraciones criticas al evitar trafico publico.

## Trade-offs: ECS Fargate vs EKS

Ventajas de ECS Fargate para este caso:
- Menor complejidad operativa inicial y menor carga de plataforma.
- Time-to-market mas rapido para equipos de delivery con foco en producto.
- Integracion nativa simple con ALB, IAM roles for tasks, CloudWatch y autoscaling.

Costos/limitaciones frente a EKS:
- Menor flexibilidad para patrones avanzados de scheduling y custom controllers.
- Menor portabilidad de practicas Kubernetes nativas.
- Menos control fino de runtime comparado con un cluster EKS maduro.

Criterio de seleccion:
- Para una organizacion que prioriza velocidad de adopcion, gobierno estandar y bajo overhead operacional, ECS Fargate es la opcion mas eficiente.
- EKS se evalua cuando la escala de equipos/plataforma justifica operar Kubernetes como capacidad central.

## Relacionado

- [02 - Infrastructure as Code](02-infrastructure-as-code.md)
- [03 - Security and Compliance](03-security-and-compliance.md)
- [04 - Disaster Recovery and Resilience](04-disaster-recovery-and-resilience.md)
- [05 - Cost Optimization and FinOps](05-cost-optimization-finops.md)
- Referencia de governance CI/CD del portfolio: [enterprise-cicd-governance-blueprint](https://github.com/andminin-engineering/enterprise-cicd-governance-blueprint)
