# Gerenciador SIEG — Certificadora MV

Sistema de gerenciamento de certificados digitais com dashboard, notificações por e-mail e exportação Excel.

## Arquivos do Projeto

```
gerenciador-sieg/
├── main.py              # Aplicação FastHTML (código principal)
├── requirements.txt     # Dependências Python
├── Dockerfile           # Container para Cloud Run
├── .dockerignore        # Arquivos ignorados no build
├── Procfile             # Fallback para Buildpacks
└── README.md            # Este arquivo
```

## Deploy no Google Cloud Run

### Opção 1 — Com Dockerfile (RECOMENDADO)

```bash
# 1. Navegue até a pasta do projeto
cd gerenciador-sieg

# 2. Deploy direto (usa o Dockerfile automaticamente)
gcloud run deploy gerenciador-sieg \
  --source . \
  --region us-central1 \
  --allow-unauthenticated \
  --set-env-vars="LOGIN_USER=mvtec2026,LOGIN_PASSWORD=MV@@2026"
```

### Opção 2 — Build manual + Deploy

```bash
# 1. Build da imagem
gcloud builds submit --tag gcr.io/SEU_PROJETO/gerenciador-sieg

# 2. Deploy
gcloud run deploy gerenciador-sieg \
  --image gcr.io/SEU_PROJETO/gerenciador-sieg \
  --region us-central1 \
  --allow-unauthenticated \
  --port 8080
```

## Variáveis de Ambiente (opcionais)

Configure no Cloud Run para habilitar envio de e-mails:

| Variável         | Descrição                    | Padrão          |
|-----------------|------------------------------|-----------------|
| `LOGIN_USER`    | Usuário de login             | `mvtec2026`     |
| `LOGIN_PASSWORD`| Senha de login               | `MV@@2026`      |
| `SMTP_SERVER`   | Servidor SMTP                | `smtp.gmail.com`|
| `SMTP_PORT`     | Porta SMTP                   | `587`           |
| `EMAIL_USER`    | E-mail remetente             | (vazio)         |
| `EMAIL_PASSWORD`| Senha do e-mail (app password)| (vazio)        |
| `SESSION_SECRET`| Chave secreta da sessão      | (auto-gerado)   |

### Configurando variáveis no Cloud Run:

```bash
gcloud run services update gerenciador-sieg \
  --region us-central1 \
  --set-env-vars="EMAIL_USER=seu@gmail.com,EMAIL_PASSWORD=sua-app-password,SMTP_SERVER=smtp.gmail.com"
```

## Executando Localmente

```bash
pip install -r requirements.txt
python main.py
# Acesse http://localhost:8080
```
