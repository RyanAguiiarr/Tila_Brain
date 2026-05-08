---
title: "Tila_Brain — Overview"
type: overview
tags: [overview, tila, brain]
last_updated: 2026-05-07
---

# Tila_Brain — Visão Geral

O **TILA** (Tecnologia Integradora de Laudos Automatizados) é uma plataforma acadêmica de assistência médica por IA que visa automatizar a geração de rascunhos de laudos radiológicos (pré-laudos) a partir de imagens DICOM e notas clínicas. Desenvolvido por Ryan Cantareli de Aguiar e Pedro Henrique Oliveira Pereira como Projeto Integrador ADS 2026, o sistema combina Spring Boot 4 (Java 21), Angular 19, PostgreSQL com pgvector, e Google Gemini via LangChain4j 0.36.2.

O **backend** está funcional para autenticação (JWT com HttpOnly cookie, BCrypt), gestão parcial de pacientes (criar, listar, buscar), e logs de auditoria. Existem 7 entidades JPA, das quais 3 possuem CRUD ativo, 3 foram criadas mas não possuem controllers/services (Exame, Laudo, ConhecimentoMedico), e 1 é placeholder vazio (Consulta). A infraestrutura de IA (LangChain4j beans para chat, embedding, e pgvector store) está configurada, mas nenhum agente, service, ou endpoint conecta esses componentes em um pipeline operacional. Foram encontrados 6 vulnerabilidades críticas de segurança (3 secrets hardcoded, 2 NPE risks, 13 campos LGPD sem proteção) e 14 divergências de convenção.

O **frontend** Angular é 100% standalone com 14 componentes (10 páginas, 4 compartilhados). A autenticação, listagem de pacientes, cadastro, e logs de auditoria funcionam com dados reais do backend. Dashboard, Centro de Laudos, e Laudo IA exibem dados mockados. A Agenda não tem backend correspondente. 57% dos componentes adotam Angular Signals, nenhum usa lazy loading, e todas as URLs de API são hardcoded.

As **3 prioridades imediatas** são: (1) mover todos os secrets para variáveis de ambiente, (2) corrigir os `.get()` sem verificação que causam NPE, e (3) implementar o LaudoService + LaudoController para conectar a entidade Laudo — que já existe — ao pipeline de IA — que já tem os beans configurados.
