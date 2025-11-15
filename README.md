# backstage-platform-rocks

Backstage como plataforma de engenharia para o projeto "It's On Fight".

Este repositório contém a base do workshop/instalação do Backstage (aplicação monorepo padrão do Backstage) com um exemplo de app em `back-its-on` e pacotes auxiliares.

**Objetivo**

- Fornecer instruções para instalar e executar o Backstage localmente.
- Documentar os passos iniciais para usar o Backstage como plataforma de engenharia do projeto "It's On Fight".

**Sumário**

- **Pré-requisitos**
- **Instalação rápida (desenvolvimento)**
- **Comandos úteis**
- **Configuração e próximos passos**
- **Depuração e problemas comuns**

**Pré-requisitos**

- Node.js 20 ou 22 (consistente com `package.json`).
- Yarn (este repositório usa `yarn@4`/Berry — recomendamos usar a versão especificada localmente).
- Git
- Docker (opcional, para execução em containers ou build de imagens)

Se necessário, instale o Node via nvm:

```bash
# instale nvm (se não tiver)
curl -fsSL https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash
# depois, instale Node 20
nvm install 20
nvm use 20
```

Para usar Yarn (v4) globalmente:

```bash
# recomendo usar o instalador do Yarn ou corepack
corepack enable
corepack prepare yarn@stable --activate
```

**Instalação rápida (desenvolvimento local)**

1. Clone o repositório:

```bash
git clone <URL-DO-REPO> backstage-platform-rocks
cd backstage-platform-rocks/back-its-on
```

2. Instale dependências (no diretório `back-its-on`):

```bash
yarn install
```

3. Inicie a aplicação Backstage em modo de desenvolvimento:

```bash
yarn start
```

Isso deve iniciar os serviços do Backstage (frontend e backend). A interface normalmente ficará disponível em:

```
http://localhost:3000
```

Observação: o script `start` do monorepo usa `backstage-cli repo start`, que gerencia os workspaces e scripts do Backstage. Para comandos individuais, veja a seção "Comandos úteis" abaixo.

**Comandos úteis**

- `yarn start` — inicia a aplicação em desenvolvimento (monorepo `back-its-on`).
- `yarn build:backend` — build do backend.
- `yarn build:all` — build de todos os pacotes (frontend + backend).
- `yarn test` — executa os testes do repo.
- `yarn new` — scaffolding de novos plugins ou componentes via `backstage-cli`.

Alguns scripts podem ser executados por workspace, por exemplo:

```bash
cd back-its-on/packages/app
yarn workspace app start
```

ou para backend:

```bash
cd back-its-on/packages/backend
yarn workspace backend start
```

**Configuração básica**

- Arquivo principal de configuração: `back-its-on/app-config.yaml` (ou `app-config.local.yaml` para overrides locais). Edite-o para configurar conexões com catálogos, autenticação (OAuth, LDAP), bancos de dados e integracões.
- Se estiver usando providers de autenticação (e.g. GitHub), ajuste variáveis de ambiente e registre a aplicação no provedor.

Exemplo de variável de ambiente comum:

```bash
export DATABASE_CLIENT=pg
export DATABASE_URL=postgres://user:pass@localhost:5432/backstage
```

**Deploy e produção (visão geral)**

- Para testes locais com Docker, crie imagens com `yarn build-image` e use `docker-compose` ou `kubernetes` para orquestrar.
- Em produção, recomenda-se Kubernetes com um ingress e configuração segura de segredos (sealed-secrets / HashiCorp Vault / AWS Secrets Manager).
- Pense em escalar o backend separadamente do frontend e em adicionar um proxy reverso (NGINX/ingress) + TLS.

**Boas práticas para usar o Backstage neste projeto**

- Centralize componentes e plugins customizados dentro de `back-its-on/packages` e `back-its-on/plugins`.
- Use o `catalog` para registrar serviços, componentes e equipes do "It's On Fight".
- Configure templates do `software-template` para padronizar criação de novos serviços.

**Próximos passos sugeridos**

1. Configurar autenticação (GitHub/GitLab/Google) para permitir login.
2. Registrar os primeiros componentes no `catalog` (via `catalog-info.yaml`).
3. Criar templates de scaffolding para novos microserviços e pipelines CI/CD.
4. Preparar um ambiente de staging em Kubernetes.

**Depuração e problemas comuns**

- Erro de versões do Node: confirme a versão com `node -v`.
- Dependências faltando: rode `yarn install` no diretório `back-its-on`.
- Porta 3000 ocupada: modifique `app-config.yaml` ou libere a porta.

Se precisar, adicione prints dos logs do backend (`packages/backend`) para identificar problemas de inicialização.

**Contribuindo**

- Fork, crie uma branch feature e abra PRs descrevendo o que foi alterado.
- Mantenha o `lint` e `prettier` conforme configurado no `package.json`.

---

Se quiser, eu aplico este README no repositório agora (faço o patch) e depois posso:

- adicionar um exemplo de `app-config.local.yaml` com valores de exemplo;
- criar um `docs/` com checklist de onboarding;
- ou gerar `.vscode` com instrução para Copilot em pt-BR.

Diga qual (ou quais) ações quer que eu execute a seguir.
