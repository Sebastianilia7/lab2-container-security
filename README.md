# Lab 2 – Container Security & Supply Chain Security

Labben visar hur man bygger, scannar, härdar och signerar en container, genererar SBOM och testar nätverkssäkerhet med NetworkPolicies i Kubernetes.

## Vad som gjorts

- Byggt en **sårbar Dockerfile** (medvetet osäker)
- Scannat med **Trivy** (visar kritiska sårbarheter)
- Härdat Dockerfile (non-root, minimal base image, rensat cache etc.)
- Trivy-scan på härdad image (färre eller inga kritiska sårbarheter)
- Genererat **SBOM** (SPDX-format): `sboms/vulnerable-app.spdx.json`
- Signerat image med **Cosign** (key-based) och verifierat signaturen
- Testat **NetworkPolicies** i Kubernetes:
  - Frontend → Backend: tillåtet
  - Frontend → MongoDB: blockerat
- Reflektion och dokumentation inkluderad

## Sårbar Dockerfile (exempel)

```dockerfile
FROM node:18
WORKDIR /app
COPY . .
RUN npm install
USER root
EXPOSE 3000
CMD ["node", "src/index.js"]
