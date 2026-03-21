Lab 2 – Container Security & Supply Chain Security

I denna labb har vi arbetat med säkerhet i hela container-livscykeln och Kubernetes-policyer. Syftet har varit att förstå hur man identifierar och hanterar sårbarheter i containerbilder, härdar dem, genererar en SBOM (Software Bill of Materials) samt säkerställer korrekt nätverkspolicy i Kubernetes.

Projektbeskrivning

Först byggde vi en medvetet sårbar Docker-image och scannade den med Trivy. Skanningen visade flera kritiska säkerhetsbrister, vilket gav en tydlig bild av riskerna med osäkra images. Därefter skapade vi en härdad version av imagen, med minimal base image, non-root user och rensade cache-filer. En ny Trivy-scan på den härdade imagen visade att de flesta kritiska sårbarheter försvunnit.

Vi genererade också en SBOM i SPDX-format, som dokumenterar alla komponenter i imagen. Detta underlättar framtida säkerhetsgranskningar och gör det lättare att följa upp CVE:er och compliance-krav. Imagen signerades sedan med Cosign för att säkerställa integritet och verifierbarhet.

NetworkPolicies i Kubernetes

För att testa nätverkssäkerhet definierade vi NetworkPolicies som styr trafiken mellan pods:
	•	Frontend → Backend: tillåtet
	•	Frontend → MongoDB: blockerat

Dessa policys verifierades genom att testa anslutningar, vilket visade att otillåtna anslutningar nekades och tillåtna fungerade som förväntat.

Reflektion

Labben har tydligt visat hur viktigt det är att hantera säkerhet i hela container-livscykeln, från base image till applikation. Sårbara images kan snabbt exponera kritiska CVE:er, medan härdade images med uppdaterade paket och non-root-användare minskar riskerna avsevärt.

SBOM är ett värdefullt verktyg eftersom det ger fullständig insyn i komponenterna i en image och gör det enklare att snabbt agera vid upptäckta sårbarheter.

Gatekeeper och NetworkPolicies tvingar teamet att följa säkerhetspolicys innan pods skapas, vilket gör säkerhet till en integrerad del av arbetsflödet och minskar risken för misstag.

Exempel på Dockerfile

Lab 2 – Container Security & Supply Chain Security

I denna labb har vi arbetat med säkerhet i hela container-livscykeln och Kubernetes-policyer. Syftet har varit att förstå hur man identifierar och hanterar sårbarheter i containerbilder, härdar dem, genererar en SBOM (Software Bill of Materials) samt säkerställer korrekt nätverkspolicy i Kubernetes.

Projektbeskrivning

Först byggde vi en medvetet sårbar Docker-image och scannade den med Trivy. Skanningen visade flera kritiska säkerhetsbrister, vilket gav en tydlig bild av riskerna med osäkra images. Därefter skapade vi en härdad version av imagen, med minimal base image, non-root user och rensade cache-filer. En ny Trivy-scan på den härdade imagen visade att de flesta kritiska sårbarheter försvunnit.

Vi genererade också en SBOM i SPDX-format, som dokumenterar alla komponenter i imagen. Detta underlättar framtida säkerhetsgranskningar och gör det lättare att följa upp CVE:er och compliance-krav. Imagen signerades sedan med Cosign för att säkerställa integritet och verifierbarhet.

NetworkPolicies i Kubernetes

För att testa nätverkssäkerhet definierade vi NetworkPolicies som styr trafiken mellan pods:
	•	Frontend → Backend: tillåtet
	•	Frontend → MongoDB: blockerat

Dessa policys verifierades genom att testa anslutningar, vilket visade att otillåtna anslutningar nekades och tillåtna fungerade som förväntat.

Reflektion

Labben har tydligt visat hur viktigt det är att hantera säkerhet i hela container-livscykeln, från base image till applikation. Sårbara images kan snabbt exponera kritiska CVE:er, medan härdade images med uppdaterade paket och non-root-användare minskar riskerna avsevärt.

SBOM är ett värdefullt verktyg eftersom det ger fullständig insyn i komponenterna i en image och gör det enklare att snabbt agera vid upptäckta sårbarheter.

Gatekeeper och NetworkPolicies tvingar teamet att följa säkerhetspolicys innan pods skapas, vilket gör säkerhet till en integrerad del av arbetsflödet och minskar risken för misstag.

Exempel på Dockerfile

Sårbar Dockerfile

"FROM node:18
WORKDIR /app
COPY . .
RUN npm install
USER root
EXPOSE 3000
CMD ["node", "src/index.js"]"

Härdad Dockerfile

"FROM node:18-alpine
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production && rm -rf /var/cache/apk/* /tmp/*
COPY . .
USER node
EXPOSE 3000
CMD ["node", "src/index.js"]"
