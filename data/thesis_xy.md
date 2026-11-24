In the context of this thesis, machine learning and graph neural network models (such as the VAE-GNN and GNN decomposition algorithms) formulate the microservice boundary identification problem by mapping an input feature representation $X$ (representing the monolithic system's components and multi-channel dependencies) to a target outcome $y$ (representing microservice cluster assignments or boundary decisions).

Below are concrete examples of how $X$ and $y$ are defined within this research framework:

## 1. Examples of $X$ (Input Features & Graph Representation)

In this thesis, the primary architectural unit (node $v \in V$) is a Django application module (e.g., `payments`, `fiscalization`, `subscriptions`, `homework_balancer`). The input matrix $X$ and graph structure $G = (V, E, W)$ encode multi-channel coupling features extracted across code, ORM, and framework layers:

* **Node Feature Vector ($X_i$ for a Django App $v_i$):**
  Quantitative Attribute Vector: A feature vector summarizing the architectural profile of app $v_i$: 
  $$\mathbf{x}_{\text{payments}} = [\text{Imports: } 12, \text{ FKs: } 29, \text{ Celery Tasks: } 8, \text{ DRF Serializers: } 15, \dots]$$ 
  *(e.g., capturing the app's share of the system's 435 ForeignKey fields, 54 Celery tasks, and 202 DRF serializers).*

* **Typed Edge Features ($\mathbf{t}(e_{ij})$ for dependencies between apps $v_i$ and $v_j$):**
  A binary type vector $\mathbf{t}(e) = (t_{\text{code}}, t_{\text{orm}}, t_{\text{framework}})$ recording which channels contribute to coupling:
  * **Example 1 — Fully Coupled Edge:** $\mathbf{t}(e_{\text{payments, fiscalization}}) = (1, 1, 1)$, indicating cross-module code calls ($t_{\text{code}}=1$), database ForeignKey / SQL JOIN links ($t_{\text{orm}}=1$), and Django signals or Celery task calls ($t_{\text{framework}}=1$).
  * **Example 2 — Loose Code-Only Edge:** $\mathbf{t}(e_{\text{users, products}}) = (1, 0, 0)$, indicating a simple Python import without direct database foreign keys or framework signal handlers.

* **Graph Structure / Adjacency Matrix ($A$ or $W$):**
  A matrix $W$ where $W_{ij} \in [0, 1]$ represents the normalized combined weight of all code calls, ORM relationships, and framework triggers connecting application $i$ to application $j$.

*In the VAE-GNN architecture, the encoder takes $(X, W)$ to compress each Django app into a low-dimensional latent embedding vector $Z_i = \text{Encoder}(X_i, W)$.*

## 2. Examples of $y$ (Target Labels & Microservice Boundaries)

The target variable $y$ represents the desired service partitioning or cluster assignment against which models are trained, clustered, or evaluated:

* **Categorical Cluster / Service Assignment ($y_i$):**
  A discrete label indicating which microservice cluster Django app $v_i$ belongs to:
  * $y_{\text{payments}} = \text{"Financial/Billing Service"}$
  * $y_{\text{fiscalization}} = \text{"Financial/Billing Service"}$
  * $y_{\text{homework\_balancer}} = \text{"Educational Core Service"}$
  * $y_{\text{homework\_iterations}} = \text{"Educational Core Service"}$

* **Pairwise Same-Service Relation ($y_{ij}$):**
  A binary indicator used for pair-based evaluation (Precision, Recall, $F_1$ score):
  * $y(\text{payments}, \text{fiscalization}) = 1$ (both belong in the same microservice).
  * $y(\text{payments}, \text{homework\_balancer}) = 0$ (belong in separate microservices).

* **Soft / Fuzzy Membership Vector ($\mathbf{y}_i$):**
  When using Fuzzy c-Means clustering on the learned embeddings, $\mathbf{y}_i$ is a vector of probabilities/belongingness values across $k$ clusters:
  $$\mathbf{y}_{\text{app\_settings}} = [0.15 \text{ (Billing)}, 0.20 \text{ (Education)}, 0.65 \text{ (Shared Infrastructure)}]$$
  identifying cross-cutting or shared utility applications that sit on service boundaries.

* **Ground-Truth Expert Decomposition ($y^{\text{expert}}$):**
  The benchmark decomposition created independently by domain experts based on business capabilities and Django design patterns, against which the model's outputs $\hat{y}$ are validated.
