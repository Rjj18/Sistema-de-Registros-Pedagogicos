# Sistema de Registro Pedagógico

## Contexto do Projeto
Este projeto foi desenvolvido para solucionar a fragmentação dos registros de atividades docentes na **PMSP**, substituindo fluxos informais (como Padlet e WhatsApp) por um aplicativo **Offline-first**. O foco central é a baixa fricção para o professor e a centralização de dados estruturados para a coordenação pedagógica.

---

## 🎥 Demonstrações Visuais

> **[INSIRA AQUI O GIF 01: FLUXO DE ENTRADA]**
> *Demonstração da Landing Page e acionamento do botão "Nova Entrada" via Action centralizada.*

> **[INSIRA AQUI O GIF 02: CAPTURA E SYNC]**
> *Demonstração do preenchimento do formulário e da sincronização automática em tempo real.*

> **[INSIRA AQUI O GIF 03: DASHBOARD DO COORDENADOR]**
> *Demonstração da View de Galeria com os filtros de segurança aplicados por perfil de usuário.*

---

## 🏗️ Arquitetura de Dados

### Tabela Principal: `Registros`
Estrutura de colunas conforme configurado no motor AppSheet:

| Nome da Coluna | Tipo | Chave (Key) | Rótulo (Label) | Lógica / Fórmula |
| :--- | :--- | :---: | :---: | :--- |
| **ID** | Text | ✅ | ☐ | `UNIQUEID()` |
| **Data_Hora** | DateTime | ☐ | ✅ | `NOW()` (Initial Value) |
| **Docente** | Text | ☐ | ☐ | `USEREMAIL()` |
| **Turma** | Enum | ☐ | ☐ | Lista de seleção para validação de dados |
| **Nome Atividade** | Text | ☐ | ☐ | Título descritivo do registro |
| **Descrição Atividade** | Text | ☐ | ☐ | Relato pedagógico detalhado |
| **Imagem** | Image | ☐ | ✅ | Armazenamento via caminho relativo no Drive |

### Tabelas Auxiliares
* **`Professores` (Lookup):** Tabela essencial para converter o `USEREMAIL()` em nomes amigáveis, contornando a depreciação da função `USERNAME()` por provedores de autenticação.
* **`Home` (Singleton):** Tabela de configuração para ativos estáticos da tela inicial (Landing Page), desacoplando o conteúdo da interface.

---

## 🧠 Lógica de Sistema e UX

* **Resiliência de Identificação:** Implementação de `LOOKUP` para garantir a captura correta do nome docente em contas institucionais, evitando campos vazios.
* **Navegação de Baixa Fricção:** Utilização da fórmula `LINKTOFORM()` na Landing Page para reduzir cliques e simplificar a experiência do usuário.
* **Modo Offline-first:** Ativação da opção `Store content for offline use` para garantir que os registros sejam salvos mesmo em áreas da escola com sinal de Wi-Fi instável.
* **Segurança (Row-Level Security):** Configuração de **Slices** com lógica `OR` para permitir visualização total para a coordenação e visualização restrita (apenas os próprios registros) para o corpo docente.

---

## 📁 Gestão de Ativos (Storage)
As imagens são organizadas no Google Drive utilizando caminhos relativos e prefixos estruturados para facilitar auditorias e automações futuras:
* **Fórmula do Prefixo do Arquivo:** `CONCATENATE([Turma], "_", [Docente], "_", TEXT(NOW(), "YYYY-MM-DD"))`

---

## 🤖 Desenvolvimento Assistido por IA
Este aplicativo foi desenvolvido em um modelo de **co-criação Homem-Máquina**.
* **Arquiteto de IA:** Gemini 3 Flash.
* **Papel da IA:** Estruturação do banco de dados, depuração de fórmulas, redação da documentação técnica e otimização de UX.
* **Validação Humana:** Todas as decisões arquiteturais foram validadas pelo Coordenador Pedagógico Roger Oliveira, aplicando conhecimentos de Engenharia de Computação (UNIVESP) para garantir aderência à realidade escolar da PMSP.

---

## ⚖️ Licença e Disclaimer

### Licença MIT
Copyright (c) 2026 Roger Aparecido Silva de Oliveira.
* Livre para cópia, modificação e distribuição.
* O software é fornecido "no estado em que se encontra", sem garantias de qualquer tipo.

### Disclaimer de Privacidade (LGPD)
O uso deste aplicativo deve respeitar a LGPD no ambiente escolar. Recomenda-se que os registros fotográficos foquem em produções, atividades e processos pedagógicos, evitando a exposição desnecessária de rostos de estudantes sem a devida autorização institucional prévia.
