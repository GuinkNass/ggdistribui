# ggdistribui
GG Distribuidora · Sistema Web + ERP Interno

Sistema completo da GG Distribuidora para operação comercial, prospecção de novos lojistas e controle interno (ERP), projetado para rodar em hospedagem compartilhada com deploy via FTP.

O projeto entrega:

Um site público profissional e chamativo em ggdistribui.com.br

Uma área restrita com login (ERP interno)

Banco de dados MySQL dedicado

Exportação de relatórios (CSV/PDF)

Geração de orçamentos

Controle de estoque, pedidos, clientes

Sem depender de Node, Docker ou containers no servidor final.
A pilha é PHP + MySQL + HTML + CSS + JS puro.

🚀 Visão Geral

O sistema é dividido em duas partes:

Site público (marketing e vendas B2B)

Página institucional moderna e agressiva visualmente

Foco em atrair mercados, farmácias, papelarias etc.

Captação de leads (lojas interessadas em revender)

Área restrita (ERP interno da GG Distribuidora)

Acesso via login

Cadastro e gestão de clientes

Cadastro de produtos

Estoque e movimentações

Pedidos e orçamentos

Relatórios CSV e PDF

Dashboard com visão geral da operação

Esse ERP interno tem visual mais simples, direto e funcional, otimizado pra rotina (não pra ser bonito pro público).

🏗 Arquitetura de Pastas

A estrutura final que sobe via FTP no hosting é:

public_html/
├── public/
│   ├── index.php          # Landing page comercial (home)
│   ├── sobre.php          # Quem somos / proposta de valor
│   ├── produtos.php       # Vitrine dos produtos e kits
│   ├── contato.php        # Form para captar lojas interessadas
│   ├── login.php          # Login para área restrita (ERP)
│   ├── dashboard.php      # Tela inicial interna após login
│   ├── clientes.php       # CRUD clientes
│   ├── estoque.php        # Controle de estoque e movimentações
│   ├── pedidos.php        # Pedidos de venda
│   ├── orcamentos.php     # Geração de orçamento em PDF
│   ├── relatorios.php     # Exportações CSV/PDF
│   └── assets/
│       ├── css/
│       │   ├── base.css   # Tema público (visual marketing)
│       │   └── erp.css    # Tema interno (uso operacional)
│       ├── js/
│       │   ├── app.js
│       │   └── erp.js
│       └── img/
│           ├── logo-light.svg
│           ├── logo-dark.svg
│           ├── background-hero.jpg
│           └── (outros assets visuais)
│
├── api/
│   ├── bootstrap.php      # Carrega config, sessão, segurança básica
│   ├── auth.php           # Login/logout, valida sessão
│   ├── clientes.php       # Endpoints CRUD de clientes
│   ├── estoque.php        # Entrada/saída de estoque
│   ├── pedidos.php        # Criação e listagem de pedidos
│   ├── relatorios.php     # Gera CSV
│   ├── orcamento_pdf.php  # Gera PDF de orçamentos
│   ├── contato.php        # Recebe leads do formulário público
│   ├── security.php       # Proteções CSRF / sanitização
│   ├── schema.sql         # Script de criação do banco
│   └── seed.php           # Popular dados iniciais (admin, clientes demo etc.)
│
├── storage/
│   ├── logs/
│   │   └── app.log        # Log de uso/erros
│   ├── tmp/
│   └── orcamentos/        # PDFs gerados
│
├── config.php             # Conexão PDO com MySQL e constantes globais
├── .htaccess              # HTTPS, bloqueios, segurança de diretórios
└── README-INSTALACAO.txt  # Guia rápido de instalação no host


Essa estrutura é pensada para hospedagem padrão (cPanel / Locaweb / HostGator / etc.), onde você faz upload via FTP e pronto.

🗄 Banco de Dados

O sistema usa um banco MySQL dedicado:

Host: distgg.mysql.dbaas.com.br

Database: distgg

Usuário: distgg

Senha: G@g2025

(Esses dados serão carregados via config.php)

Tabelas principais

O script api/schema.sql cria as tabelas:

users: controle de acesso (email, senha hash, role)

clientes: dados comerciais dos lojistas / PDVs

produtos: catálogo de itens distribuídos

estoque_movimentos: histórico de entradas / saídas

pedidos e pedido_itens: pedidos de venda

orcamentos: orçamentos gerados e caminho do PDF salvo

leads: contatos vindos do site público (lojas interessadas)

Também existe api/seed.php, que:

Cria um usuário admin inicial (admin@ggdistribui.com.br / GG@2025Demo)

Insere clientes e produtos de exemplo

Popular estoque e pedidos de teste
Isso ajuda a validar o sistema logo após o deploy.

Importante: depois de rodar seed.php a primeira vez, esse arquivo deve ser removido do servidor por segurança.

🔐 Autenticação e Segurança

Login feito em login.php, validado via api/auth.php

Sessão PHP com cookie HttpOnly e SameSite=Lax

Senha salva usando password_hash() e verificada com password_verify()

Rotas internas (dashboard.php, clientes.php, etc.) só carregam se houver sessão válida

api/security.php deve implementar:

Gerador e verificador de token CSRF para formulários internos

Sanitização básica de entrada para evitar SQL injection

.htaccess faz:

Forçar HTTPS

Bloquear listagem de diretórios

Bloquear acesso direto a storage/

Desligar display_errors em produção

🎨 UI e Design
Site Público (marketing)

Visual forte, escuro, estilo tech/SaaS, com acentos neon roxos (#7d5fff)

Fundo escuro (#0f0f1e)

Cartões com bordas suaves, sombra e glow leve

Botões “call to action” chamativos (“Quero revender”, “Fale com atendimento”)

Seções:

Hero principal (headline + CTA)

Como funciona o modelo de consignação

Para quem é (mercados, farmácias, papelarias etc.)

Benefícios financeiros para o lojista

Prova social e regiões atendidas

Formulário de contato / WhatsApp

Esse conteúdo comunica a proposta da GG Distribuidora: expositores prontos, giro rápido, margem boa para o lojista e reposição sem burocracia.

Área Restrita (ERP)

Layout mais funcional e limpo

Sidebar fixa com navegação interna

Dashboard com métricas rápidas (clientes ativos, itens em estoque baixo, pedidos abertos hoje)

Telas em modo tabela/formulário

CSS separado em erp.css

Foco em produtividade, não marketing

📊 Recursos do ERP Interno

Dashboard interno

KPIs rápidos de operação

Clientes

Criar, editar, remover

Buscar por nome, CPF/CNPJ

Guardar telefone/WhatsApp

Produtos e Estoque

Cadastro de produtos (SKU, preço, categoria)

Controle de saldo atual

Movimentação manual de estoque (entrada / saída)

Histórico de movimentações

Pedidos

Criar pedido vinculando cliente + itens

Status do pedido: aberto / faturado / entregue

Cálculo de total

Orçamentos

Gerar orçamento para um cliente com produtos e valores

Exportar PDF com logo e dados da empresa

Salvar o PDF gerado em storage/orcamentos/ para histórico

Relatórios

Exportar CSV de clientes, produtos, pedidos

Baixar orçamentos em PDF

Possível exportação XLSX futuramente

Leads comerciais (público → interno)

Toda mensagem enviada via contato.php
cai na tabela leads.

Isso alimenta prospecção de novos pontos de venda.

🔄 Fluxo de Deploy

Este projeto foi desenhado para ser publicado sem etapas complexas.

1. Subir arquivos

Faça upload de toda a estrutura (public_html/) para sua hospedagem via FTP/SFTP

2. Configurar config.php

Ajustar host do banco, usuário, senha

Configurar nome da empresa, CNPJ, telefone que aparecerão nos PDFs

3. Criar banco e rodar o schema

Crie o banco MySQL na hospedagem (caso ainda não exista)

Rode o conteúdo de api/schema.sql

Você pode colar no phpMyAdmin

4. Rodar seed.php uma única vez

Acesse no navegador:
https://SEU_DOMINIO/api/seed.php

Isso cria o usuário admin e dados fake de teste

Depois APAGUE seed.php do servidor

5. Permissões de pasta

Dê permissão de escrita (chmod 0777 ou via painel da hospedagem) para:

storage/logs/

storage/tmp/

storage/orcamentos/

Isso é necessário para gerar PDF, logs e arquivos temporários.

6. Login inicial

Vá até https://SEU_DOMINIO/login.php

Entre com:

Email: admin@ggdistribui.com.br

Senha: GG@2025Demo

Você deve ser redirecionado para dashboard.php

✅ Checklist rápido pós-deploy

 O site público (index.php) abre sem erro

 Formulário de contato envia e aparece em leads

 Consigo logar no ERP via login.php

 Dashboard mostra dados iniciais

 Consigo cadastrar um novo cliente

 Consigo lançar entrada/saída de estoque

 PDF de orçamento é gerado e salvo em storage/orcamentos/

 Consigo exportar CSV em relatorios.php

 storage/ não está acessível publicamente via navegador

 seed.php já foi removido do servidor

🔒 Segurança / Produção

Ativar HTTPS no domínio ggdistribui.com.br

Garantir que display_errors do PHP esteja OFF em produção

Nunca deixar seed.php online depois da configuração

Proteger /api/ para endpoints sensíveis:

todos devem validar sessão

todos devem validar CSRF nos POST

Logs não podem armazenar senha nem dados sensíveis em texto puro

🧭 Roadmap Futuro

Versões futuras planejadas:

Emissão de NFe/NFSe

Módulo de vendedores/comissão

App mobile / PWA

Dashboard com gráficos

Exportação XLSX além de CSV

Multiunidade (controle de estoque por ponto físico)

Suporte a nota de consignação / demonstrativo para o lojista

📌 Status

Versão: 1.0.0
Domínio alvo: ggdistribui.com.br
Ambiente alvo: hospedagem PHP + MySQL com deploy via FTP
Banco de dados padrão: distgg (MySQL)

📞 Contato interno

Empresa: GG Distribuidora

Email comercial: contato@ggdistribuidora.com.br

WhatsApp comercial: +55 11 91234-5678

Resumo final

Esse repositório contém tudo que a GG Distribuidora precisa para:

Vender mais (site público bonitão e otimizado pra convencer lojista a revender)

Controlar a operação (ERP interno simples, direto e seguro)

Rodar sem dor de cabeça técnica (PHP + MySQL + FTP)

Subiu via FTP, configurou config.php, rodou schema.sql + seed.php, deu permissão em storage/ — está no ar. 🟣💼
