# iis: ML-based microservice boundary identification for Django monoliths

Master Thesis , VGTU: *Research on the Application of Machine Learning in Software Modernization*.

## Target company

Edflix a mid-size online school (EdTech) that teaches students through a web platform with homework, subscriptions and online payments.

The platform is built as a Django monolith with PostgreSQL. Over the years it has grown into many tightly coupled apps (`payments`, `fiscalization`, `subscriptions`, `homework_balancer` and others), connected through imports, ForeignKeys and SQL JOINs, Django signals and Celery tasks. This makes releases risky, slows down new features and prevents teams from working independently.

The company needs to move to microservices, but the main question is where to draw service boundaries. Manual analysis of hundreds of ForeignKeys, Celery tasks and serializers is slow and subjective. My method builds a unified dependency graph (code + ORM + framework), learns embeddings with a VAE-GNN, clusters them and proposes candidate boundaries. Generative AI names the clusters and explains why a boundary was suggested, and developers review the result. The company gets a reproducible, evidence-based starting point for decomposition and a lower risk of migration.

## Materials

- Examples of (X, y): [data/thesis_xy.md](data/thesis_xy.md)
- Process model (Mermaid): [data/process-model.md](data/process-model.md)
- UML diagram (PlantUML), where GenAI is integrated:
  - ![UML EdTech](data/UML_EdTech.png)
