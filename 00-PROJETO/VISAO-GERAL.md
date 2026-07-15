# Visão geral do ChatPro

## Objetivo

ChatPro é um projeto acadêmico para criar uma plataforma de atendimento por WhatsApp. Não há finalidade comercial imediata, e o desenvolvimento e a apresentação devem priorizar custo zero.

## Escopo planejado

- Conexão futura com WhatsApp por QR Code.
- Envio e recebimento futuro de textos, áudios, imagens, vídeos e documentos.
- Mensagens rápidas e CRM em fases futuras.
- Painel web e PWA para atendimento.
- Aplicativo móvel/APK futuro consumindo os mesmos serviços.
- Preparação arquitetural para hospedagem e integrações pagas futuras, sem contratá-las ou implementá-las agora.

## Limites e segurança

Baileys é a integração inicialmente planejada, mas não é uma API oficial do WhatsApp. Ela deverá permanecer isolada atrás de uma abstração própria para permitir substituição futura.

Testes devem usar somente números, contas, conversas e dados expressamente autorizados. Credenciais, sessões e dados pessoais não devem ser versionados.

## Estado atual

A fase documental inicial foi concluída. Não existem ainda aplicação, conexão com WhatsApp, banco, autenticação, CRM ou APK implementados.
