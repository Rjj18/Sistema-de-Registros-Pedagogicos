# Sistema de Registro Pedagógico

## Visão Geral
Aplicativo **offline-first** desenvolvido no **AppSheet** para centralizar e padronizar os registros de atividades docentes na PMSP, substituindo fluxos informais (por exemplo, Padlet e WhatsApp) por um processo estruturado, auditável e alinhado a boas práticas de dados.

## Objetivos
- Centralizar registros pedagógicos em uma única fonte de verdade
- Reduzir fricção de uso (menos cliques, fluxo guiado)
- Garantir funcionamento em ambientes com conectividade instável
- Aplicar segurança por perfil e por linha (row-level security)
- Facilitar auditoria e evolução do modelo de dados

## Escopo
- Cadastro de registros (atividade, descrição, turma e evidências)
- Armazenamento de imagens em Google Drive por caminho relativo
- Visualização por perfis (docentes e coordenação)
- Sincronização automática quando houver conectividade

## Demonstrações
Substitua os placeholders abaixo por GIFs ou links para vídeos curtos.

- Demonstração 01 — Fluxo de entrada: Landing Page e ação "Nova Entrada"
- Demonstração 02 — Captura e sincronização: preenchimento do formulário e sync
- Demonstração 03 — Painel da coordenação: galeria com filtros por perfil

## Arquitetura de Dados (AppSheet)

### Tabela Principal: Registros
Estrutura de colunas conforme configurado no AppSheet.

| Coluna | Tipo | Key | Label | Lógica / Configuração |
| --- | --- | :---: | :---: | --- |
| ID | Text | Sim | Não | UNIQUEID() |
| Data_Hora | DateTime | Não | Sim | NOW() (Initial value) |
| Docente | Email/Text | Não | Não | USEREMAIL() |
| Turma | Enum | Não | Não | Lista de seleção para validação |
| Nome_Atividade | Text | Não | Não | Título do registro |
| Descricao_Atividade | LongText | Não | Não | Relato pedagógico |
| Imagem | Image | Não | Sim | Armazenamento por caminho relativo no Drive |

Notas recomendadas:
- Padronize nomes de colunas (snake_case) e evite espaços para facilitar expressões e integrações.
- Defina validações e valores iniciais para reduzir inconsistências.

### Tabelas Auxiliares
- Professores (Lookup): converte USEREMAIL() em nome amigável, mitigando limitações/depreciações de funções de nome.
- Home (Singleton): configurações e ativos estáticos da Landing Page, desacoplando conteúdo da interface.

## Lógica de Sistema e UX
- Identificação resiliente: uso de LOOKUP para obter o nome do docente a partir do e-mail.
- Navegação de baixa fricção: LINKTOFORM() na Landing Page para iniciar rapidamente um novo registro.
- Offline-first: habilitar Store content for offline use para permitir captura sem conectividade.
- Segurança (Row-Level Security): Slices com lógica OR para:
  - Coordenação: acesso ampliado
  - Docentes: acesso apenas aos próprios registros

## Gestão de Ativos (Google Drive)
As imagens são organizadas por prefixos estruturados para facilitar auditoria e automações.

- Prefixo sugerido:
  - CONCATENATE([Turma], "_", [Docente], "_", TEXT(NOW(), "YYYY-MM-DD"))

Recomendações:
- Evite dados sensíveis no nome do arquivo.
- Considere mascarar e-mails ou substituir por IDs quando necessário.

## Privacidade e Conformidade (LGPD)
O uso deve respeitar a LGPD no ambiente escolar.
- Priorize evidências de produções/atividades, evitando exposição desnecessária de estudantes.
- Defina política de retenção e permissões de acesso.
- Oriente usuários sobre consentimento e boas práticas de captura.

## Desenvolvimento Assistido por IA
Projeto desenvolvido em modelo de co-criação humano-máquina.
- Arquiteto de IA: Gemini 3 Flash
- Papel da IA: apoio na estruturação do modelo de dados, depuração de expressões, documentação e UX
- Validação humana: decisões validadas pelo Coordenador Pedagógico Roger Oliveira

## Licença
Distribuído sob licença MIT.

Copyright (c) 2026 Roger Aparecido Silva de Oliveira.
