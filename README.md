# Minha Fazenda

Plataforma de Gestão de Fazenda moderna, intuitiva e "animadora", substituindo pranchetas por um aplicativo móvel gamificado e um painel web corporativo.

## Visão Geral
* **App Mobile**: React Native (Expo) - Focado na coleta de dados com funcionamento Offline-First para pastos sem internet.
* **Web Admin**: Next.js - Dashboard analítico para conversão alimentar e projeção de lucros.
* **Backend**: Firebase (Cloud Firestore, Auth, Functions e Hosting).

## Estrutura do Projeto
```
├── .github/workflows/  # CI/CD pipelines
├── src/                # Código-fonte principal
├── tests/              # Testes (unit, integration, firebase rules)
├── Docs/               # Documentação completa e metodologia (Ideação, Arquitetura)
└── README.md
```

## Como rodar

**Pré-requisitos**: Node.js, Firebase CLI.

```bash
# Clone e entre na pasta
git clone ...
cd "Minha Fazenda"

# Instale dependências
npm install

# Setup das variáveis locais
cp .env.example .env
```

## Documentação Completa
Consulte a pasta `Docs/` para detalhes completos de arquitetura, banco, modelo de negócios e o cronograma do projeto em `CHRONICLE.md`.
